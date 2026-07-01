# DOCS_SOURCE_OF_TRUTH.md

> Phase 1 fact-extraction for Nito (publicly *Z Inference*) developer docs.
> Built by mining the `protocol-z/inference-gateway` repository. Code is the source of truth.
> Every claim cites `file:symbol`. Where a number or behavior could not be verified in code, it is marked **TODO** and also recorded in `DOCS_GAPS.md`.

**Analyzed commit:** `6ada3ccf7413aaae6e01803a1291aa805a9e071b` (`main`, 2026-06-29, "[codex] add request-shape telemetry (#494)")
**Repo:** `github.com/protocol-z/inference-gateway` (Go)
**Extraction date:** 2026-06-30
**OpenAPI file:** none present in repo. All schemas below are inferred from Go structs and handlers; any consumer doc that needs a machine-readable contract is a TODO until `docs/planning/openapi.yaml` lands.

---

## 0. Headline reality check (read this first)

The outline was written ahead of the code. Mining the repo at this commit surfaced large divergences. The most load-bearing:

1. **No `T1/T2/T3/T4` tier scheme exists.** Privacy is three stored tiers (`anon`, `private`, `tee`) plus five request modes (`ordinary`, `anon`, `private`, `tee`, `e2e-tee`). Selection is a **model-ID suffix** (`:tee` / `:e2e-tee`), not a `:tier` parameter or a request field. (`internal/models/models.go:34-46`)
2. **The TEE attestation gateway is a *baseline* verifier, not a hardware-quote verifier.** It checks field-stability and binding (`report_data = signing_address + nonce`); it explicitly does NOT verify Intel TDX quotes, NVIDIA GPU evidence, cert chains, CRLs, or TCB. Full assurance is the client "verify-yourself" path. (`docs/architecture/tee-attestation.md:52-54`)
3. **Many outline endpoints return 404.** `/v1/responses`, `/v1/messages`, `/v1/messages/count_tokens`, `/v1/files`, `/jobs/*` are all wired to `notFoundRouteHandler`. (`internal/server/routes.go:141-147`)
4. **Several retention numbers in the outline are not in code** (idempotency-replay 24h, debug-capture 7d, saved-chat 30d/90d). They are TODO/SME, not facts. (see §10)
5. **A `/api/v1/chat/fusion` multi-model endpoint exists** that the outline only hints at, and an `x_search` (X/Twitter) tool the outline never mentions.

A full divergence ledger is in §11.

---

## 1. Endpoints (route table)

Source: `internal/server/routes.go: serverRoutes`. Routes carry a surface (`gateway` / `admin`) and optional gates (Stripe, billing-admin, etc.). Two public version prefixes: first-party `/api/v1/...` and OpenAI-compat bare `/v1/...`.

### Inference & model surface (gateway)
| Method | Path | Handler | Notes |
|---|---|---|---|
| POST | `/api/v1/chat/completions`, `/v1/chat/completions` | `apiChatCompletionsHandler` | Primary chat. Both paths share the handler. (`routes.go:84,137`) |
| POST | `/api/v1/chat/fusion` | `apiChatFusionHandler` | Multi-model fan-out + synthesis. No `/v1` alias. (`routes.go:85`) |
| POST | `/api/v1/embeddings`, `/v1/embeddings` | `apiEmbeddingsHandler` | (`routes.go:88,138`) |
| POST | `/api/v1/images/generations`, `/v1/images/generations` | `apiImageGenerationsHandler` | FAL-backed text-to-image. (`routes.go:89,139`) |
| POST | `/api/v1/attachments/parse` | `apiAttachmentParseHandler` | Multipart file→text. (`routes.go:86`) |
| POST | `/api/v1/augment/text-parser` | `apiTextParserHandler` | Multipart file→text/tokens. (`routes.go:87`) |
| GET | `/api/v1/tee/attestation` | `apiTEEAttestationHandler` | (`routes.go:90`) |
| GET | `/api/v1/models`, `/v1/models` | `apiModelsHandler` | (`routes.go:136,140`) |
| GET | `/api/v1/model-groups` | `apiModelGroupsHandler` | (`routes.go:94`) |

### Account / auth / org / billing (gateway, browser-session unless noted)
Auth & OAuth: `/oauth/authorize`, `/oauth/authorize/approve|deny`, `/oauth/token`, `/oauth/revoke`, `/oauth/register`, `/oauth/device_authorization`; `/.well-known/oauth-authorization-server`, `/.well-known/openid-configuration`, `/.well-known/oauth-protected-resource(/*)`; `/api/v1/auth/status`, `/api/v1/auth/social/providers`; session pages under `/login`, `/signup`, `/auth/google|x/*`, `/password/*`, `/logout`. (`routes.go:122-170`)
Session/CSRF: `GET /api/v1/session`, `GET /api/v1/csrf`, `GET /api/v1/me/storage-key`. (`routes.go:91-92,115`)
Account security: `GET /api/v1/account/security`; TOTP setup/confirm/disable; identities email/wallet/disable; password; session revoke; oauth-token revoke (all `POST`, CSRF). (`routes.go:104-113`)
Orgs: `GET|POST /api/v1/orgs`, nested `/api/v1/orgs/*` dispatched by `apiOrgRouteHandler` to members / invitations / `api/overview` / free-tier / `billing/balance` / api-keys. (`routes.go:102-103,313-335`)
Billing/funding: `GET /api/v1/billing/checkout/products`, `POST /api/v1/billing/checkout/sessions`, `POST /api/v1/billing/portal/sessions`, `POST /api/v1/billing/x402/top-up`, `POST /webhooks/stripe` (inbound). (`routes.go:97-100,69`)
Other: `GET|POST /api/v1/referrals`, notifications, backups (manifest/catalog/presign), share-links. MCP transport mounted dynamically at `/mcp`. (`routes.go:101,116-121,95-96,148`)

### Explicitly UNSUPPORTED (mounted → 404 via `notFoundRouteHandler`)
`/v1/responses`, `/v1/responses/*`, `/v1/messages`, `/v1/messages/count_tokens`, `/v1/files`, `/v1/files/*`, `/jobs/*`. (`routes.go:141-147,349`)

### Outline endpoints that DO NOT EXIST (no route)
`GET /api/v1/generation`, `GET /api/v1/models/{id}/endpoints`, `GET /api/v1/providers`, `POST /v1/funding/intents/{id}`, `POST /v1/funding/stripe/checkout-sessions`, any flat `/api/v1/billing/balance|usage`, any audio/video endpoints, image edits/variations. (see §11)

---

## 2. Chat & Text generation

Handler `apiChatCompletionsHandler` (`internal/server/api_chat.go`). Strict JSON decode (`decodeStrictJSON`, `DisallowUnknownFields`), size-capped by `apiMaxRequestBytes` (default 4 MiB). Multipart/form-data also accepted (a `request` JSON field + image attachments).

### Request — `apiChatRequest` (`internal/server/api_chat.go: apiChatRequest`)
- `messages` `[]apiChatRequestMessage` — required; empty → `messages is required` (`internal/inference/validation.go: ValidateChatRequest`).
- `model` string, optional — empty falls back to `h.defaultModel`. Router mode requires `provider/model` form.
- `stream` bool; `stream_options.include_usage` bool.
- `temperature` `*float64`, `top_p` `*float64`, `max_tokens` `*int64`, `max_completion_tokens` `*int64` — all optional, nil passthrough.
- `reasoning` `*apiReasoning` (see §7).
- `prompt_cache_key` string (≤256 bytes; model must support), `cache_control` raw JSON object (model must support).
- `web_search` (bool or object), `web_fetch` `*bool` (see §7).
- `tools` `[]apiChatTool`, `tool_choice` `*apiChatToolChoice` (see below).
- `user` string.
- Duplicate feature keys rejected: `request body contains duplicate feature fields`.

`apiChatRequestMessage`: `role`, `content` (string OR content-part array), `tool_call_id`, `tool_calls`.

### Roles & content parts
Roles accepted by inference validation: `system`, `developer`, `user`, `assistant`, `tool` (`internal/inference/contracts.go: Role`; `validateMessage`). Note `developer` is accepted (passthrough, no special handling).
Content-part kinds in code: `text`, `image`, `file` only (`ContentPartKind`). Attachment sources: `inline_base64`, `data_url`, `url`, `provider_file_id`, `provider_file_uri` (`AttachmentSource`).
- `text` → `{text, cache_control?}`.
- `image_url` → `{image_url:{url, detail?, media_type?}}`; raw base64 + `media_type` assembled into a data URL (`imageURLWithMediaType`). Model must support image inputs.
- `file` / `input_file` → `file_url` / `file_data` / `filename`; **`file_id` is rejected** (`...file_id is not supported`). Files parsed server-side inline (text/pdf/docx/pptx/xlsx/xls) via the same parser as the attachments endpoint.
- `audio` / `audio_url` / `input_audio` → rejected (`audio inputs are not supported`). `video*` → rejected. `document` → rejected for chat. (`api_chat.go: apiChatContentPartToInference`)
- URL attachments must be https, no credentials, no private/loopback/metadata hosts (`validateAttachmentURL`).

### Non-streaming response — `apiChatResponse`
`id`, `object:"chat.completion"`, `created` (unix s), `model`, `choices[]` (always one), `system_fingerprint` (always serialized `null`), `usage`, optional `web_search`, optional `free_tier`.
- `apiChatChoice`: `index`, `message`, `finish_reason`.
- `apiChatMessage`: `role`, `content` (always present), `tool_calls?`, `reasoning_details?`.
- `apiUsage`: `prompt_tokens`, `prompt_tokens_details?` (`cached_tokens`, `cache_creation_input_tokens`), `cache_read_input_tokens?`, `cache_write_input_tokens?`, `completion_tokens`, `reasoning_tokens?`, `total_tokens`, `server_tool_use?` (`web_search_requests`, `web_search_results`, `web_fetch_requests`).

### Finish reasons — exactly four (`internal/inference/contracts.go: FinishReason`)
`stop`, `length`, `tool_calls`, `error`. Emitted verbatim from provider; empty serializes as `null` in chunks.

### Tool calling
Request `apiChatTool`: `type`, `function{name, description?, parameters}`, server-tool `parameters?`, `cache_control?`. Function tools need unique `function.name` and object `parameters`. Server/hosted tool types: `web_search`, `web_fetch`, `x_search`, `code_interpreter` (+ provider-prefixed variants). (`internal/server/api_chat_tools.go`, `internal/inference/contracts.go: ToolType`)
`tool_choice`: string (`auto`/`none`/`required`) or object `{type, function:{name}}`. (`apiChatToolChoice`)
Response: `choices[].message.tool_calls` `[{id, type:"function", function:{name, arguments}}]`; finish reason `tool_calls`. The gateway routes tool declarations/calls/results but does **not** execute caller-defined function tools.

### Status codes & errors
200 success; 405 (non-POST, sets `Allow`); 400 (decode/validation/feature/attachment). Provider/inference errors mapped by `inference.StatusCode` (see §9). All responses `Cache-Control: no-store`.

### `/api/v1/chat/fusion` — `apiChatFusionHandler` (`internal/server/api_chat_fusion.go`)
Asks 2–3 models the same prompt concurrently, then one synthesis ("judge") pass merges them. **Non-streaming/batched only.** Request `apiFusionRequest`: `messages`, `models[]` (2–3 distinct, `fusionMinModels=2`/`fusionMaxModels=3`), `temperature?`, `top_p?`, `max_tokens?`, `user?`, `web_search?`, `web_fetch?`, `tools?` (hosted web tools only — function tools rejected). Response `apiFusionResponse`: `object:"fusion.completion"`, `judge`, `synthesis`, `synthesis_prompt_tokens`, `synthesis_tokens`, `synthesis_attestation?`, `models[]` (per-model `apiFusionModelResult`: id/model/provider/output/tokens/latency_ms/finish_reason/web_search?/attestation?/error?), `free_tier?`. Privacy via model suffix; cannot mix TEE and non-TEE; `:e2e-tee` rejected. Per-participant budget 150 s.

### E2E-TEE chat constraints
When `X-Z-TEE-*` E2EE headers are present: streaming refused (`streaming is not supported with E2E-TEE`); reasoning, tools, prompt caching, and multi-part messages also rejected (`internal/inference/validation.go: validateTEEEncryption`). Non-streaming only, NEAR/Phala providers only.

---

## 3. Streaming (SSE)

Triggered by `stream:true` (not E2E-TEE) → `streamAPIChatCompletion` (`internal/server/api_chat.go`).
- Transport headers (`prepareSSE`, `internal/server/sse.go`): `Content-Type: text/event-stream; charset=utf-8`, `Cache-Control: no-store, no-cache`, `X-Accel-Buffering: no`. 200 written immediately, then a heartbeat + flush.
- Frame format (`writeSSEEvent`): `data: <json>\n\n`. Heartbeat: `: keep-alive\n\n` every `sseHeartbeatInterval = 15s`. Termination: `data: [DONE]\n\n`.
- Chunk shape `apiChatChunk`: `id`, `object:"chat.completion.chunk"`, `created`, `model`, `choices[]` (`index`, `delta`, `finish_reason` `*string` nil until terminal). `delta`: `role?` (assistant, sent once), `content?`, `tool_calls?`, `reasoning_details?`. Optional `usage?`, `web_search?`, `error?`, `free_tier?`.
- Usage chunk (`stream_options.include_usage:true`): a trailing chunk with empty `choices:[]` and populated `usage`, just before `[DONE]`.
- Tool-call streaming: `ToolCallStarted` → `{index, id, type:"function", function:{name}}`; `ToolCallArgsDelta`/`Completed` → `{index, function:{arguments}}` fragments; completed args echoing a streamed index are suppressed.
- Reasoning streaming: `reasoning_details:[{type, summary, id, format, index}]` on reasoning deltas with non-empty summary.
- Mid-stream error: a chunk with top-level `error:{message,code,param}` and `choices:[{index:0, delta:{}, finish_reason:"error"}]` (`writeChatStreamError`). If `open()` fails before any byte is written, a full HTTP error is sent instead (`writeInferenceError`).

---

## 4. Embeddings, Multimodal, Image generation, File parsing

### Embeddings — `apiEmbeddingsHandler` (`internal/server/api_embeddings.go`)
Request `apiEmbeddingsRequest`: `input` (string or `[]string`; token-ID arrays rejected), `model?` (falls back to `h.defaultEmbeddingModel`), `encoding_format?` (`""`/`float` only — `base64` NOT supported), `dimensions?` `*int`, `user?`.
Validation (`internal/embeddings/validation.go`): `MaxInputs=96`, per-entry `MaxInputBytes=32 KiB`, `dimensions` bounded 1–3072.
Response `apiEmbeddingsEnvelope`: `object:"list"`, `model`, `data[]` (`{object:"embedding", index, embedding:[]float32}`), `usage{prompt_tokens, total_tokens}`.
Default embedding model native ID `Qwen/Qwen3-Embedding-0.6B` (NEAR & Phala). **Per-model output dimensions are NOT encoded in the gateway** — the provider returns the vector length at runtime. TODO for any per-model dimension table.

### Image generation — `apiImageGenerationsHandler` (`internal/server/api_images.go`)
Request `apiImageGenerationRequest`: `model?`, `prompt` (required), `n?` (0–4), `size?` (e.g. `1024x1024`), `quality?`, `style?`, `response_format?` (`url` default / `b64_json`), `user?`.
Response `apiImageGenerationResponse`: `created`, `model`, `data[]` (`url?`, `b64_json?`, `revised_prompt?`), `usage{prompt_tokens, output_images, total_tokens}`.
Provider: **FAL** (`internal/provider/fal/fal.go`). Default model `fal/fal-ai/z-image/turbo` ("Z-Image Turbo"). Privacy header `X-Fal-Store-IO: 0`. `quality`/`style` rejected for FAL. Output always PNG. Billed by pixels.

### Attachments / file parser
`POST /api/v1/attachments/parse` (`internal/server/api_attachments_parse.go`): **multipart required**. Fields: `file`, `trust_tier` (`anon|private|tee`; `e2e-tee` unsupported), `parse_mode` (`""|server|auto`). Size cap `apiMaxParseAttachmentBytes` (default 10 MiB) → 413 on oversize. Accepted kinds: `text`, `pdf`, `docx`, `pptx`, `xlsx`, `xls`; **images rejected** ("send directly to an image-capable model"). Response: `text`, `mime_type`, `file_kind`, `bytes`, `page_count?`, `parsed_pages?`, `truncated?`, `warnings[]`. Caps: 300k output runes, 80 PDF pages, 10 s PDF timeout. Billed as `z-protocol/document-parse`, 1 unit.
`POST /api/v1/augment/text-parser` (`apiTextParserHandler`): same upload/parser; extra `response_format` (`json` default / `text`). JSON response `{text, tokens}` (tokens ≈ runes/4). Different response shape from `/attachments/parse`.

### Multimodal input in chat
Supported input modalities: **image** (PNG/JPEG/WebP via `image_url` or multipart; limits `apiChatMultipartMaxImages=10`, 5 MiB/image, 50 MiB combined, 2048px long edge) and **document** (PDF/docx/pptx/xlsx/xls/text, parsed inline). Everything else (audio in, video in) is rejected. Generation: **image only** (FAL).

### Files API
`/v1/files`, `/v1/files/*` → 404 (`notFoundRouteHandler`). NOT IMPLEMENTED.

---

## 5. Models & Providers

### Two ID spaces (the outline's `provider/model[:tier]` conflates them)
- **Physical scoped ID** = `provider/providerModelID` (e.g. `openrouter/openai/gpt-5.5`). `models.ScopedModelID` / `ParseScopedModelID` (`internal/models/models.go:400,409`).
- **Logical public ID** = `canonicalModelID[:suffix]` where suffix is **only `:tee` or `:e2e-tee`** (anything else rejected). `ParseLogicalModelIDForPublicMode` (`models.go:485`). Router error string: `logical model id must use <model>, <model>:tee, or <model>:e2e-tee format` (`internal/server/logical_models.go:81`). Legacy suffixes `standard|zdr|e2ee|anon|private` recognized as invalid.

### Catalog = runtime-discovered, not a static list
Models are discovered from provider APIs and upserted into Postgres `supported_models` (`models.Syncer.SyncProvider` → `Store.SyncProviderModels`). No hardcoded catalog; only per-provider default-model constants. Dev seed triggers live sync; the only fixed model is localdev `z-protocol/file-attachment-echo`. Listing per-model is therefore a TODO (no static list to publish).
Model-card fields (`models.SupportedModel`/`DiscoveredModel`): provider, model, providerModelID, canonicalModelID, displayName, description, modelType (`chat|embedding|image|audio|video|other`), privacyTier, teeProvider, attestationAvailable, attestationSubject, e2eeSupported, supportsImageInputs, freeTierEligible, available, active, timestamps. **Pricing and context length are NOT on the model card** — joined from the `billing` package at response time.

### Providers (`internal/provider/*`, registry `internal/provider/registry/registry.go`)
| Provider | const | Serves via | Privacy enforcement |
|---|---|---|---|
| xAI / grok | `xai` | OpenAI Responses adapter | `Store: false` forced (`internal/provider/openairesponses/request.go:33`) |
| OpenRouter | `openrouter` | OpenAI Chat adapter | `ZDR = RequireZeroDataRetention && !hasServerTools`; embeddings/images force `ZDR:true`; non-ZDR models never imported (`internal/provider/openrouter/request.go:118`) |
| NEAR | `near` | oaiadapter | ed25519 E2EE headers; `AttestationClient`; domain-based attestation |
| Phala | `phala` | oaiadapter | appid-based attestation |
| fal | `fal` | native HTTP | `X-Fal-Store-IO:0`; image-only |
| localdev | `z-protocol` | local echo | dev only |
`oaiadapter` and `openairesponses` are shared adapters, not serving providers.

### Model groups / routing / fallback
`GET /api/v1/model-groups` (`apiModelGroupsHandler`), `?capability=chat|embeddings|images`. Grouping collapses physical rows sharing a `canonicalModelID` into logical options per public mode. Backend selection `chooseLogicalRoute` (`logical_models.go:910`): available > free-tier > (ordinary) private-over-anon > price-known > cheapest est. cost > provider priority > name. On a private route, `RequireZeroDataRetention` is set (drives OpenRouter ZDR).

### Privacy-tier → model mapping
Stored tiers: `anon` (default for all providers), `private`, `tee`. OpenRouter: anthropic/google/openai owners → `anon`, else `private`. NEAR/Phala: base `anon`, upgraded to `tee` when attestation/appid present (+ `E2EESupported` for attested chat). xAI/fal/localdev: `anon`.

---

## 6. Privacy tiers & TEE attestation

### Tiers as implemented (`internal/models/models.go:34-46`)
Internal storage: `anon`, `private`, `tee`. Public modes: `ordinary`, `anon`, `private`, `tee`, `e2e-tee`. Both `tee` and `e2e-tee` collapse to internal tier `tee`; `ordinary` → empty tier. Outline mapping: T1→`anon`, T2→`private` (routing sets ZDR), T3→`tee` mode, T4→`e2e-tee` mode (`tee` tier + `E2EESupported` + `TEEEncryption` headers). **No `Tier1..Tier4` symbols exist; document the named tiers and note the marketing labels separately.**
Invariants (`validatePrivacyCapabilities`, `models.go:730`): anon/private must carry no TEE metadata; `tee` requires `TEEProvider ∈ {near, phala}`; attestation requires tier `tee`; E2EE requires tee + attestation + subject; NEAR does not require a subject, others do.

### Per-call selection
Via the `:tee` / `:e2e-tee` model-ID suffix. E2EE additionally requires the `X-Z-TEE-*` headers parsed into `TEEEncryption` (`internal/server/api_chat_e2ee.go`). E2EE only valid for NEAR/Phala; E2EE streaming rejected. For the attestation endpoint, tier is also selectable via `trust_tier` query (`tee`/`e2e-tee`).

### Attestation endpoint — `GET /api/v1/tee/attestation` (`internal/server/api_tee_attestation.go`)
Allow-listed query keys: `model` (required), `nonce`, `signing_address`, `signing_algo`, `trust_tier`. Resolves the public model to a physical attestation-capable `SupportedModel`, calls the provider's `AttestationClient`, then `verifyTEEAttestationBaseline`.
Response `apiTEEAttestationResponse`: `object:"tee.attestation"`, `model`, `routed_model?`, `provider`, `provider_model_id`, `privacy_tier`, `tee_provider`, `attestation_available`, `attestation_subject?`, `verified`, `nonce?`, `proof_format?`, `signing_algo?`, `signing_address?`, `signing_key?`, `model_public_key?`, `evidence` (raw JSON). Note `signing_key` and `model_public_key` are both populated from `attestation.ModelPublicKey` — they are aliases, not two independent keys.

### The seven-gate baseline (`docs/architecture/tee-attestation.md:32-43`; `api_tee_attestation_verification.go: verifyTEEAttestationBaseline`)
1. Provider evidence present & parseable as a JSON object.
2. Provider-declared `verified:false` marks unverified.
3. Proof format, signing algorithm, and model public key present; caller `signing_algo` (if given) must match.
4. Caller nonce matches observed provider nonce.
5. Gateway report-data binding (`report_data == signing_address + nonce`) + requested-address match.
6. Nested model-attestation binding (model ID, algo, model signing identity, public key, nonce).
7. Nested JSON payload nonce for GPU envelopes (`nvidia_payload`/`payload`/`evidence`).
**Scope limit (load-bearing):** the gateway does NOT verify Intel TDX quotes, NVIDIA GPU evidence, cert chains, CRLs, or TCB policies — those are the client's responsibility. Use "baseline-verified" / "attested", never "trustless" or "fully verified".

### Freshness binding & address-only invariant
`report_data = signing_address + nonce`, recomputed and compared case-insensitively (`verifyGatewayAttestationObject`, `:117`). Anti-replay is **caller-driven**: the caller supplies a fresh random nonce; an omitted nonce permits an empty-binding report (flag this — not unconditional replay protection). Binding uses the **signing address only**; entries without a model public key do not count as a binding (`verifyModelAttestationBinding:157-163`). Gateway and model signing addresses are kept separate; baseline verification does not require them to be equal.

### Per-provider attestation
Shared endpoint `attestation/report`, shared parser `oaiadapter.ParseAttestationResponse`. NEAR sends subject as **`domain`**, default proof format `near-attestation-report-v1`, subject optional. Phala sends subject as **`appid`**, default proof format `redpill-attestation-report-v1`, subject required.

### Verify-yourself path
(a) Raw `evidence` + signing address + model public key + proof format + echoed nonce are returned for independent client verification. (b) Reference verifier `tools/tee-attestation-smoke/main.go` (`make tee-attestation-smoke`) generates a 32-byte nonce, GETs the endpoint, and runs independent checks (status 200, envelope, TEE metadata, nonce echo, evidence present, proof metadata, baseline verified).

---

## 7. Features

| Feature | Status | How invoked / notes |
|---|---|---|
| Web search | **EXISTS** | Top-level `web_search` (bool/object) or `tools:[{type:web_search}]`. Gateway-native Brave pipeline (`internal/websearch/`) with a rules engine; provider-routed for OpenRouter/xAI/NEAR/Phala. Billed as `brave/llm_context` & `brave/web_search` (`internal/websearchbilling`). |
| Web fetch | **EXISTS** | `web_fetch` field or tool. OpenRouter/NEAR/Phala only. |
| Code interpreter | **EXISTS (xAI only)** | `tools:[{type:code_interpreter}]` → `xai:code_interpreter` passthrough. No gateway-native sandbox. |
| x_search (X/Twitter) | **EXISTS (xAI only)** | `tools:[{type:x_search}]`. Not in outline. |
| Datetime tool | **NOT FOUND** | No tool type/handler. |
| File parser | **EXISTS (REST, not chat tool)** | `/api/v1/attachments/parse` & `/api/v1/augment/text-parser` (see §4). |
| Reasoning | **EXISTS** | `reasoning{enabled, effort(low/medium/high), summary(auto/concise/detailed), max_tokens, exclude}` or model-ID feature suffix. Output `reasoning_details` + `usage.reasoning_tokens`. |
| Prompt caching | **EXISTS (passthrough)** | `prompt_cache_key` + `cache_control` forwarded to provider; cache-hit accounting in usage. Gateway does not store prefixes. |
| Idempotency | **EXISTS** | `Idempotency-Key` header, must be UUID. Scoped to **billing dedup** (`AuthorizeInferenceRequest.IdempotencyKey`); no response-replay cache observed in the server layer. |
| Structured outputs | **NOT EXPOSED** | `inference.ChatRequest.ResponseFormat` exists and is forwarded to providers, but the public chat API never populates it; no JSON-schema validation. |
| Response caching | **NOT FOUND** | Only prompt-cache passthrough. |
| Message transforms | **NOT FOUND** | No request-level `transforms` field. |
| BYOK | **NOT FOUND** | Provider keys come from gateway config only. |
| Guardrails (injection/moderation) | **NOT FOUND (chat)** | Only outbound web-search query redaction of secrets/PII (`internal/websearch/redaction.go`). No chat-prompt moderation or injection classifier. |
| App attribution / request metadata | **NOT WIRED** | `ChatRequest.Metadata` exists but the HTTP surface only wires `user`. No `HTTP-Referer`/`X-Title`. |
| Generation metadata retrieval | **NOT FOUND** | No `GET /api/v1/generation` route. |
| Outbound webhooks (consumer) | **NOT FOUND** | Only inbound Stripe webhooks + an internal Slack model-alert. No per-customer event delivery. |
| MCP transport | Present (out of scope) | `/mcp` Streamable HTTP; OAuth `z_mcp_` tokens, scope `mcp:inference`. SDK/server docs deferred. |

---

## 8. Auth, accounts, keys, orgs

### Token prefixes (`internal/auth/tokens.go`)
- `z_sk_` — org API key (`APIKeyTokenPrefix`).
- `nito_cli_` — current OAuth CLI access token; `z_cli_` — legacy CLI token (still accepted).
- `z_mcp_` — OAuth MCP access token.
- **No `sk-` and no bare `z_`.** Tokens = 32 random bytes base64url; stored as HMAC-SHA256(token, `SESSION_SECRET`). No raw token stored.

### API keys
Org-scoped only. Endpoints (via `apiOrgAPIKeysHandler`): `GET /api/v1/orgs/{id}/api-keys` (list), `POST .../api-keys` (create → `{token, id}`, raw token once), `POST .../api-keys/{keyID}/revoke`. Owner/admin only. **No GET-single, no update/PATCH, no HTTP DELETE** (delete = soft revoke). **No per-key scopes/permissions** and **no "management/elevated" key type** — privilege is purely org RBAC (owner/admin/member). Per-key spend caps (`daily|monthly|overall`, nanos) are the only per-key control; cap breach force-expires the key.

### OAuth (PKCE S256 + device code)
`code_challenge_method` must be `S256`. Closed scope allowlist: `openid`, `cli:inference`, `mcp:inference`, `profile:org` (`offline_access` rejected). Clients `nito-cli`, `z-mcp` (loopback redirect required); dynamic registration mints `mcp-client-*` (public, `token_endpoint_auth_method=none`). Token issuance: `nito_cli_` for CLI/auth-code, `z_mcp_` when a resource is present (MCP, must match `cfg.MCP.ResourceURI`). Access-token TTL 24h, auth-code 10m, device-code 10m, poll 5s. `/oauth/token` supports `authorization_code` and device-code grants → `{access_token, token_type:"Bearer", expires_in, scope, client_id, user_id, org_id}`. `/api/v1/auth/status` is OAuth bearer introspection (not the cookie session).

### Social, session, CSRF
Social providers: `google`, `x` (`GET /api/v1/auth/social/providers`); Google = code exchange, X = PKCE. Session cookie `ig_session` (TTL 7d, `SameSite=lax`, `Secure` outside dev). `GET /api/v1/session` returns auth/anonymous state. CSRF = double-submit cookie `ig_csrf` vs `X-CSRF-Token` header, constant-time compare.

### Account security
`GET /api/v1/account/security` → identities, password capability, social, TOTP, sessions, oauth tokens, api-key orgs. TOTP setup/confirm/disable (AES-stored secret); identity link email/wallet/disable; password change (12–72 bytes, bcrypt, may require MFA); session revoke (not current); oauth-token revoke.

### Orgs & members
`GET|POST /api/v1/orgs`; nested members/invitations/`api/overview`/free-tier/`billing/balance`/api-keys. Roles `owner`/`admin`/`member` (`requireOrgRole`). Invitation target kinds: email / evm / solana.

### Rate limits (`internal/ratelimit/`)
Policy-table driven; **per-key values are global group defaults, not stored per key**. Scopes: global, ip, org, api_key, oauth_token, session, user, oauth_client, mcp_*, subject, command. Selected `inference_generate` defaults: org 600/min, api_key 120/min, session 120/min; in-flight org 25 / api_key 10. `inference_count` api_key 300/min; `model_list` api_key 600/min. **Only `Retry-After` is emitted on 429 — no `X-RateLimit-*` headers.** Limiter infra errors fail-open except for a fail-closed set (auth_interactive, inference_generate/count, mcp_*, oauth, org_admin, spa_api_account_org).

---

## 9. Errors, status mapping, headers, request IDs, config

### Error envelope (`internal/server/api_chat.go: apiErrorBody`)
`{"error":{"message","code","param?}}`. **There is NO `type` field** — the error family is carried in `code`. Writer `writeAPIError`. OAuth endpoints use RFC6749 `{"error","error_description"}`; some SPA endpoints use `{"error","code"}`.
Error kinds (`inference.ErrorKind`): `invalid_request`, `model_unavailable`, `unauthorized`, `forbidden`, `insufficient_quota`, `payment_required`, `provider_error`, `rate_limited`, `unavailable`; fallback `internal_error`.
Status mapping (`inference.StatusCode`): invalid_request→400, model_unavailable→400 (param `model`), unauthorized→401, forbidden→403, payment_required→402, insufficient_quota→429, rate_limited→429, unavailable→503, provider_error→502, unknown→500.

### Headers
Read: `Authorization: Bearer`, `Idempotency-Key` (UUID), `X-Request-ID` (≤128 chars, sanitized), `X-CSRF-Token`, `X-Z-Anonymous-ID`, `X-Forwarded-For/Proto` (trusted proxies only), `X-Z-TEE-*` (E2EE).
Set: `X-Request-ID` (echoed/generated, 16 random bytes hex), `z-env`/`z-service`/`z-version` (runtime identity), `Retry-After` (429), `Content-Type: application/json; charset=utf-8`, `Cache-Control: no-store` (inference/etc.), full security header set (`nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy`, COOP/CORP, `Permissions-Policy`, `Content-Security-Policy`; HSTS only when base URL is https).

### Base URLs & versioning
App base URL default `http://127.0.0.1:8080` (env `APP_BASE_URL`); admin surface separate (`127.0.0.1:8081`). MCP path default `/mcp`. No regional variants. Versioning = dual prefix `/api/v1` (first-party) and `/v1` (OpenAI-compat). Provider upstreams: NEAR `cloud-api.near.ai/v1`, Phala `api.redpill.ai/v1`, FAL `fal.run`, Brave `api.search.brave.com/res/v1`.

### Pagination
Minimal — `limit` query params (notifications default 20) and `window_days` time windows (usage, default 30 / max 365). **No cursor / `next_cursor` / `has_more` envelopes.**

### Config / env (`.env.example`, 331 lines)
Groups: DB (`DATABASE_URL`), object storage (`BLOB_STORAGE_*`), TigerBeetle ledger (`TIGERBEETLE_*`, optional), HTTP runtime (`HTTP_*`, `APP_*`), inference limits (`INFERENCE_API_MAX_REQUEST_BYTES=4194304`, `..._MAX_PARSE_ATTACHMENT_BYTES=10485760`, `..._STREAM_IDLE_TIMEOUT=60s`, `..._WRITE_TIMEOUT=10m`), auth/session/OAuth secrets (`SESSION_SECRET`, `STORAGE_KEY_SECRET`, `CHAT_SHARE_TOKEN_SECRETS`, `OAUTH_*`, `SOCIAL_*`), MCP (`MCP_*`), Redis (rate-limit/feature-flags), Stripe (`STRIPE_*`), billing/pricing (`BILLING_*`, including free-tier `BILLING_FREE_TIER_TEXT_PROMPTS_PER_DAY=10`, `..._IMAGE_PROMPTS_PER_DAY=15`), observability (`OTEL_*`, `SENTRY_*`), mail (`MAIL_*`), provider keys (`XAI_API_KEY`, `NEAR_API_KEY`, `OPENROUTER_API_KEY`, `PHALA_API_KEY`, `FAL_KEY` — **at least one required at startup**), web search (`BRAVE_SEARCH_API_KEY`). Circuit breaker: `PROVIDER_CIRCUIT_BREAKER_ENABLED=true`, threshold 3, cooldown 30s. Billing hold timeout 15m.

---

## 10. Data retention (privacy-critical — verify before publishing)

Source: code + `docs/standards/privacy-and-retention.md`.
- **Metadata-only default — CONFIRMED.** Prompts/completions/reasoning/tool/attachment payloads enumerated as never-persisted; telemetry restricted to a `SafeAttrs` allowlist; inference responses `Cache-Control: no-store`. Browser chat history is client-side only (encrypted IndexedDB), with a per-user AES key derived server-side via `GET /api/v1/me/storage-key` = `HMAC-SHA256(StorageKeySecret, userID)`.
- **Idempotency replay 24h — NOT CONFIRMED (TODO).** `Idempotency-Key` drives billing dedup; no 24h replay/retention window found in scanned Go. The only 24h constants are unrelated (OAuth token TTL, email token, spend-cap reservation, free-tier daily reset). May live in SQL migrations — verify with Backend.
- **Governed debug capture up to 7d — NOT CONFIRMED (TODO).** Doc says "bounded retention" with no number; no debug-capture subsystem or 7-day constant exists. The `7d` constants found are API-key expiry and session/invite TTLs.
- **Tenant saved chat 30d default / 90d cap — NOT CONFIRMED (TODO).** No server-side saved-chat store with retention; `chatshares` persists only object-key + token-hash, no TTL. `30` = usage window default; `90` = an API-key expiry option.
- **Three-distinct-keys guarantee — PARTIAL.** Three secret roles exist (`SESSION_SECRET`, `STORAGE_KEY_SECRET`, `CHAT_SHARE_TOKEN_SECRETS`), but by default the latter two are HMAC-derived from `SESSION_SECRET` (domain separation by label), independent only when each is set explicitly. Reframe as "HMAC domain-separated key hierarchy" unless deployment overrides all three. All stored credentials are HMAC/SHA-256 hashes only.
- Object-storage presign max lifetime: 15 minutes.

> Per the prompt, all retention windows, pricing, rate-limit numbers, and compliance claims are treated as **always-TODO unless confirmed by code**. The three unconfirmed retention numbers above must not be stated as fact in the docs.

---

## 11. Outline-vs-code divergence ledger (feeds DOCS_GAPS.md / DOCS_DEFERRED.md)

### Endpoints the outline lists that return 404 or do not exist
- `/v1/responses` (+ lifecycle), `/v1/messages`, `/v1/messages/count_tokens`, `/v1/files` → 404. (API Reference 5.2, 5.4, 5.15; Features Responses/Anthropic Messages) → document as **stubs**, "not implemented as of `6ada3cc`".
- `GET /api/v1/generation` (5.6) — does not exist.
- `GET /api/v1/models/{id}/endpoints`, `GET /api/v1/providers` (5.5) — do not exist.
- `POST /v1/funding/intents/{id}`, `POST /v1/funding/stripe/checkout-sessions` (5.14) — do not exist; real paths are `/api/v1/billing/checkout/sessions`, `/x402/top-up`, `/portal/sessions`.
- Flat `/api/v1/billing/balance`, `/usage`, billing events, credit history (5.13) — do not exist; balance/usage are org-scoped (`/orgs/{id}/billing/balance`, `/orgs/{id}/api/overview`).

### Capabilities the outline frames as real that the code does not implement
- Audio input, video input, audio generation, video generation, image edits/variations (Features Multimodal; API 5.4) — absent. Only image input + document input, image generation.
- Datetime server tool, structured-outputs/JSON-schema enforcement, response caching, message transforms, BYOK, chat guardrails (prompt injection / sensitive-info), app attribution, generation metadata, outbound webhooks (Features) — absent or not exposed.
- Webhooks tab (5.18) and "Webhooks (consuming Nito events)" — no outbound event system exists. Defer/stub.

### Framing corrections (exist, but not as the outline describes)
- Privacy "T1–T4" → named tiers `anon`/`private`/`tee` + `e2e-tee` mode; selection via model suffix.
- TEE attestation → baseline verifier, not full hardware verification; anti-replay is caller-nonce-driven.
- Token prefixes → `z_sk_`/`nito_cli_`/`z_cli_`/`z_mcp_`, not `sk-`/`z_`.
- API key CRUD → create/list/revoke only; no scopes; no management keys.
- Rate-limit headers → `Retry-After` only.
- Error envelope → no `type` field.
- Retention numbers → unconfirmed (TODO).

### Out of scope this pass (separate repos → DOCS_DEFERRED.md)
Native SDK, Agent SDK, CLI (`z`), MCP server. The gateway exposes the `/mcp` transport, OAuth scopes (`mcp:inference`), and token prefixes (`z_mcp_`, `nito_cli_`) as pointers for the later SDK pass, but no SDK/CLI/MCP-server pages are generated now.

---

## 12. Things to resolve first (top candidates for SME)
1. Per-model **pricing** and **context windows** (live from billing/Stripe; not in card) — Finance/Backend.
2. The three **retention windows** (idempotency 24h, debug 7d, saved chat 30d/90d) — Backend/Legal; may be in SQL migrations not scanned.
3. **Embedding output dimensions** per model — Backend.
4. **Compliance posture** (HIPAA-adjacent, SOC 2, EU residency), DPA, subprocessors, SLA — Legal/Security (always-TODO).
5. Canonical **production base URL(s)** (code default is localhost) — Product/Infra.
6. **Retry/backoff** exact retryable set & schedule (`internal/inference/retry.go` not fully enumerated) — Backend.
7. Whether **Responses API / Anthropic Messages / Files** are roadmap or cut — Product (drives stub vs delete).
8. Whether **audio/video** modalities are roadmap — Product.
9. **Playground** routes (none exist yet) — Product/Design.
10. The six **canonical diagrams** — Design.

<!-- source: internal/server/routes.go, internal/server/api_chat*.go, internal/server/api_embeddings.go, internal/server/api_images.go, internal/server/api_attachments_parse.go, internal/server/api_tee_attestation*.go, internal/server/api_orgs.go, internal/server/api_billing*.go, internal/server/rate_limit.go, internal/server/observability.go, internal/inference/contracts.go, internal/inference/validation.go, internal/inference/errors.go, internal/inference/service.go, internal/models/models.go, internal/server/logical_models.go, internal/provider/* (grok, openrouter, near, phala, fal, oaiadapter, openairesponses), internal/auth/tokens.go, internal/auth/oauth.go, internal/auth/api_keys.go, internal/ratelimit/policy.go, internal/billing/* (enforcement, ledger_holds, state_machine, stripe_*, x402, subscriptions, referrals), internal/websearch/*, internal/config/* , .env.example, docs/architecture/tee-attestation.md, docs/standards/privacy-and-retention.md -->
