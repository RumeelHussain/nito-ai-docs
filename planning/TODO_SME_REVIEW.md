# TODO (needs SME) — Review Checklist

Generated from the Nito docs. **119 distinct items** to review (the production base URL is one item repeated across 109 pages).

Each item lists the open question and the file:line where it appears. Resolving an item means confirming the fact so the TODO marker can be replaced with the real value.

---

## Product / Infra — Production base URL (HIGH PRIORITY, affects the whole site)

- **The canonical production base URL.** Every code example uses the placeholder `https://YOUR-NITO-BASE-URL`. Appears on **109 pages**. One confirmed value clears all of them in a single pass.

---

## Product (11)

- Agent SDK repository, install steps, and links
  - `resources/powered-by-z/for-developers-building-agents.mdx:29`
- Z engine specifics and the Nito-to-Z bridge
  - `resources/powered-by-z/index.mdx:28`
- Z engine specifics and the Nito-to-Z bridge for agents
  - `resources/powered-by-z/for-developers-building-agents.mdx:23`
- a concrete attestation-capable `MODEL_ID` for the `:tee` example Confirm against `GET /v1/models`.
  - `cookbook/private-ai/build-document-qa.mdx:76 (+1 more)`
- confirm `ORG_ID` and `KEY_ID` retrieval flow and any UI equivalent for teams that do not script rotation
  - `resources/best-practices/secure-api-key-handling.mdx:43`
- confirm recommended primary and fallback model pairings once canonical model IDs are published
  - `resources/best-practices/uptime-and-fallbacks.mdx:28`
- confirm the canonical location and link for the deferred-capabilities list (audio in, video in, audio generation, video generation) so this page can point readers to a single source.
  - `features/multimodal.mdx:43`
- the formal deprecation and retirement policy, including advance-notice windows and timelines. The gateway tracks availability and activation and marks models unavailable on sync, but there is no codified deprecation schedule, sunset notice, or version-deprecation mechanism at this commit. Until this is published, treat the practices below as the guidance.
  - `models/deprecations-and-lifecycle.mdx:18`
- whether audio/video generation is on the roadmap Tracked as a deferred capability.
  - `api-reference/multimodal/image-generation.mdx:90`
- whether audio/video input is on the roadmap Tracked as a deferred capability.
  - `api-reference/multimodal/pdf-input.mdx:70`

## Product/Infra (1)

- publish a status page / per-surface uptime Confirm the hosting approach, the surfaces to report on, the incident and maintenance workflow, and the subscription mechanism before any of the content below is presented as live.
  - `resources/status-page.mdx:8`

## Product/Backend (1)

- tenant saved-chat retention (30d/90d) is not present in current code; confirm roadmap
  - `privacy/data-retention/tenant-saved-chat.mdx:35`

## Product/DevRel (1)

- confirm whether outbound consumer webhooks are planned, and if so the event catalog, signing scheme, and delivery guarantees Until then, this page must not describe an outbound webhook feature as available.
  - `resources/observability/webhooks.mdx:30`

## Product/Support (1)

- confirm any gateway-side lookup or support flow that consumes a client-supplied `X-Request-ID`, so docs can tell users how to hand an ID to support
  - `resources/best-practices/privacy-safe-logging.mdx:20`

## Product/Legal (1)

- confirm the relationship between the status page and any SLA Until an SLA exists, the status page is informational only.
  - `resources/status-page.mdx:27`

## Backend (19)

- confirm the exact fields and granularity returned by the org usage overview for a cost-monitoring dashboard
  - `resources/best-practices/cost-control.mdx:32`
- confirm there is no idempotency response-replay or retention window anywhere below the server layer (for example in SQL migrations). The scanned Go shows only billing dedup and no 24h replay window; the source of truth flags the "24h replay" claim as unconfirmed
  - `features/idempotency.mdx:28`
- confirm whether the 80-page, 10-second, and 300,000-character caps apply identically to the inline `file` content-part path and the `/api/v1/attachments/parse` endpoint, or whether they differ between the two.
  - `features/multimodal/pdf-input.mdx:92`
- confirm whether the per-image 5 MiB, combined 50 MiB, 10-image, and 2048 px long-edge limits also bound images supplied as data URLs or base64 inside the `image_url` content part, or whether those numbers apply only to the multipart upload path.
  - `features/multimodal/image-input.mdx:68`
- idempotency replay/retention window The mechanism exists for billing dedup; any specific replay-retention window is unconfirmed and may live in infrastructure or SQL migrations not covered by this scan.
  - `privacy/data-retention/idempotency-replay.mdx:32`
- per-model embedding output dimensions
  - `api-reference/embeddings/vector-dimensions.mdx:39`
- publish per-model output dimensions for the default and other embedding models. This is not encoded in the gateway; the provider returns the vector length at runtime, so it must come from Backend.
  - `features/embeddings.mdx:96`
- publish the OpenAPI spec at `docs/planning/openapi.yaml`
  - `api-reference/overview/openapi-spec.mdx:8`
- the enumerated value sets for `mode`, `retrieval_mode`, `freshness`, `safesearch`, and the `location` object shape
  - `api-reference/server-tools/web-search.mdx:60`
- the exact `web_search` metadata field set on the response
  - `api-reference/server-tools/web-search.mdx:89`
- the exact accepted value sets and formats for `freshness`, `country`, `search_lang`, and `safesearch` (for example whether `freshness` uses Brave-style codes like `pd`/`pw`/`pm`/`py` or date ranges). Confirm against `internal/websearch/` before publishing literal values
  - `features/server-side-tools/web-search.mdx:68`
- the exact field layout of the response-level `web_search` object (source URLs, titles, snippets, ordering). Document against the actual serialized shape before publishing
  - `features/server-side-tools/web-search.mdx:135`
- the exact per-option field set
  - `api-reference/models-catalog/model-groups.mdx:46`
- the full field list inside `daily[]` and `models[]` entries (beyond `requests`, token counts, `customer_charge_nanos`, and the daily top model). Confirm against a live response
  - `cookbook/advanced/usage-dashboard.mdx:85`
- the gateway's exact upstream retry set and schedule (`internal/inference/retry.go` is not fully enumerated)
  - `api-reference/errors-and-rate-limits/retry-behavior.mdx:34`
- the per-model embedding output dimensions. The gateway does not encode a fixed dimension per model; the vector length is returned by the provider at runtime
  - `models/model-families.mdx:28`
- the response shape for code-interpreter output as relayed by the gateway (tool-result content parts, execution metadata, and billing treatment). Confirm against the xAI passthrough path before publishing specifics
  - `features/server-side-tools/code-interpreter.mdx:56`
- the response-level metadata shape for web fetch (fetched URL, content length, any per-page status). Confirm against the serialized response before publishing literal fields
  - `features/server-side-tools/web-fetch.mdx:57`
- the values above are illustrative. Confirm a real catalog row, its `context_window`, and `pricing` shape
  - `api-reference/models-catalog/list-models.mdx:124`

## Backend/Product (5)

- a curated table of recommended embedding models with their providers and intended use
  - `api-reference/embeddings/embedding-models.mdx:27`
- confirm published production rate limits
  - `resources/rate-limits.mdx:16`
- confirm published production rate limits and any per-deployment `Retry-After` sizing
  - `resources/rate-limits.mdx:65`
- whether there is a supported way to pin a specific physical backend (a provider-scoped `provider/model` ID) from the public API, beyond the model ID plus privacy suffix. Confirm the intended public contract
  - `models/model-id-convention.mdx:62`
- whether there is a supported way to pin a specific physical backend (for example a provider-scoped `provider/model` ID) from the public API, beyond the model ID and privacy mode. The routing logic chooses among candidates; confirm the intended public contract for explicit pinning
  - `models/backend-selection.mdx:60`

## Backend/Security (1)

- debug capture mechanism and retention bound The concept (explicit opt-in, bounded retention) is described in the privacy standard; the implementing subsystem and the specific retention duration are not verifiable in the scanned code.
  - `privacy/data-retention/governed-debug-capture.mdx:24`

## Backend/Infra (1)

- the exact published limits per deployment, if they differ from these defaults
  - `api-reference/errors-and-rate-limits/rate-limits.mdx:31`

## Infra/Backend (1)

- the exact `OTEL_*` and `SENTRY_*` variable names, required values, and which exporters/protocols are wired. Confirm against the gateway's `.env` configuration
  - `cookbook/advanced/observability-pipeline.mdx:71`

## Finance (6)

- image output pricing basis and per-model rates
  - `resources/pricing.mdx:74`
- master per-model price table and credit unit
  - `resources/pricing.mdx:16`
- server-side tool unit prices
  - `api-reference/server-tools/tool-pricing.mdx:21 (+1 more)`
- server-side tool unit prices.
  - `models/pricing.mdx:28`
- the master per-model price table and the credit-to-currency value
  - `faq/models-and-pricing.mdx:20`

## Finance/Backend (4)

- a worked example response showing the exact `pricing` object field names and sample nanos values
  - `resources/pricing.mdx:45`
- per-model pricing and context windows, and per-model embedding output dimensions, are not encoded on the model card and are not published here
  - `privacy/known-limitations.mdx:76`
- publish the per-pixel pricing (or the formula) and confirm how `n` and `size` combine into the billed amount, including any minimum charge. Pricing is not encoded in the gateway model card.
  - `features/multimodal/image-generation.mdx:73`
- the full `pricing` field set and unit definitions per model type
  - `api-reference/models-catalog/list-models.mdx:87`

## Finance/Legal (2)

- any specific privacy guarantee about payments (unlinkability of funding to requests, payer anonymity for on-chain top-ups, formal separation of payment identity from usage). State only what Legal/Finance can stand behind
  - `privacy/payment-privacy.mdx:46`
- retention and visibility of payment records and on-chain transaction references, and how long funding history is kept
  - `privacy/payment-privacy.mdx:48`

## Legal (12)

- DPA text and execution process The full Data Processing Addendum, including the signing or execution flow (self-serve acceptance, countersignature, or a portal), is not yet available and must be authored and approved by Legal.
  - `privacy/dpa.mdx:14`
- DPA text, execution process, and publication, including alignment with the subprocessor list and any GDPR/CCPA role definitions
  - `resources/trust-center/dpa.mdx:34`
- authoritative subprocessor list and change-notification process, including the correct legal role for each named provider (xAI, OpenRouter, NEAR, Phala, fal, Stripe) and any others in the data path
  - `resources/trust-center/subprocessors.mdx:30`
- categories of data processed and categories of data subjects
  - `privacy/dpa.mdx:22`
- cookie categorization, consent requirements, third-party cookie disclosures, and the final cookie notice text
  - `resources/policies/cookies.mdx:30`
- full Acceptable Use Policy text and publication, including prohibited-use categories, enforcement process, and alignment with the Terms of Service
  - `resources/policies/acceptable-use-policy.mdx:26`
- full Privacy Policy text and its publication at `/privacy`, including data-subject rights processes and any GDPR/CCPA role definitions
  - `resources/policies/privacy-policy.mdx:30`
- full Terms of Service text and its publication at `/terms`, including alignment with the Acceptable Use Policy and Privacy Policy
  - `resources/policies/terms-of-service.mdx:33`
- international transfer mechanisms, to the extent any are offered
  - `privacy/dpa.mdx:28`
- roles of the parties (controller and processor designations) and the scope of processing
  - `privacy/dpa.mdx:20`
- subprocessor list and the process for notifying and objecting to subprocessor changes. This intersects the providers behind each privacy mode described in [Provider-side enforcement](/privacy/provider-side-enforcement)
  - `privacy/dpa.mdx:24`
- the execution process: how a customer requests, reviews, and signs the DPA, and where the signed copy is stored
  - `privacy/dpa.mdx:30`

## Legal/Security (8)

- GDPR and CCPA roles and processes (controller vs processor, data-subject request handling, lawful basis), to the extent any are claimed
  - `privacy/compliance-posture.mdx:36`
- HIPAA posture. Whether Nito is "HIPAA-adjacent," supports a BAA, or handles PHI in any defined way is not established in code and is not asserted. Do not imply HIPAA alignment
  - `privacy/compliance-posture.mdx:28`
- SLA terms, including the availability target, measurement method, support-response commitments, and remedies. No uptime number may be published until this is authored
  - `resources/trust-center/sla.mdx:28`
- SOC 2. There is no SOC 2 report, audit status, or trust-services-criteria claim in the codebase. Do not state or imply SOC 2 Type I or Type II
  - `privacy/compliance-posture.mdx:30`
- any SLA or uptime commitment. None is established in code and none is asserted here
  - `privacy/compliance-posture.mdx:38`
- security measures, incident notification timelines, and audit rights
  - `privacy/dpa.mdx:26`
- subprocessor list and data-flow documentation, including the providers behind each privacy mode, framed for a compliance audience
  - `privacy/compliance-posture.mdx:34`
- the exact contractual data-processing terms behind each provider's no-retention setting, beyond the per-request technical flag enforced in code
  - `privacy/provider-side-enforcement.mdx:64`

## Security (4)

- advisory publication process, severity methodology, notification channel, and any historical advisories. No advisory content or feed may be published until this is defined
  - `resources/trust-center/security-advisories.mdx:31`
- exact E2E-TEE encryption handshake and header value construction
  - `cookbook/private-ai/end-to-end-tee-call.mdx:31`
- reporting contact and encryption key, scope definition, response-time commitments, and final safe-harbor terms
  - `resources/trust-center/responsible-disclosure.mdx:26`
- the exact invocation and any build step for the reference verifier (for example a `make` target), confirmed against the current repo
  - `privacy/tee-attestation/verify-yourself-recipe.mdx:103`

## Security / Product (1)

- confirmation of which public models map to NEAR versus Phala backends, and whether any model exposes both. The mapping is runtime-discovered, so there is no static list to publish here
  - `privacy/tee-attestation/per-provider-attestation.mdx:58`

## Security/Backend (1)

- confirm the exact construction of each `X-Z-TEE-*` header value (key encodings, supported signing algorithms, encryption version string, nonce/timestamp format) and provide a complete, runnable end-to-end example with real values
  - `privacy/per-call-privacy-modes.mdx:111`

## DevRel (23)

- GitHub, changelog, and announcement channel links
  - `resources/community.mdx:24`
- community channel links, including support forum and Discord
  - `resources/community.mdx:12`
- community showcase and project-sharing links
  - `resources/community.mdx:18`
- confirm Langfuse integration path Determine whether a first-party Langfuse export is planned, and if so what it would send.
  - `resources/observability/langfuse.mdx:37`
- publish Postman/Bruno collection files
  - `sdks/postman-bruno.mdx:10`
- verify the current Discord API/library setup against its official docs
  - `cookbook/bots/discord-bot.mdx:8`
- verify the current OpenClaw gateway/provider setup against its official docs
  - `cookbook/bots/openclaw-gateway.mdx:8`
- verify the current Slack app/API/SDK setup against its official docs
  - `cookbook/bots/slack-assistant.mdx:8`
- verify the current Telegram Bot API/library setup against its official docs
  - `cookbook/bots/telegram-bot.mdx:8`
- verify the current WhatsApp messaging API/provider setup against its official docs
  - `cookbook/bots/whatsapp-assistant.mdx:8`
- verify the exact current configuration for CrewAI against its official docs
  - `sdks/framework-integrations/crewai.mdx:37`
- verify the exact current configuration for LangChain and LangGraph against its official docs
  - `sdks/framework-integrations/langchain-langgraph.mdx:35`
- verify the exact current configuration for LiteLLM (SDK and proxy) against its official docs
  - `sdks/framework-integrations/litellm.mdx:35`
- verify the exact current configuration for LlamaIndex (LLM and embeddings) against its official docs
  - `sdks/framework-integrations/llamaindex.mdx:35`
- verify the exact current configuration for Mastra against its official docs
  - `sdks/framework-integrations/mastra.mdx:36`
- verify the exact current configuration for Pydantic AI against its official docs
  - `sdks/framework-integrations/pydantic-ai.mdx:38`
- verify the exact current configuration for the Vercel AI SDK against its official docs
  - `sdks/framework-integrations/vercel-ai-sdk.mdx:39`
- verify the exact current settings path/config for Claude Code against its official docs, including whether the running version supports an OpenAI-compatible endpoint at all
  - `sdks/editor-integrations/claude-code.mdx:18`
- verify the exact current settings path/config for Codex against its official docs, including whether the running version targets Chat Completions or the Responses API
  - `sdks/editor-integrations/codex.mdx:18`
- verify the exact current settings path/config for Continue.dev against its official docs, including the configuration key names for an OpenAI-compatible model entry
  - `sdks/editor-integrations/continue-dev.mdx:18`
- verify the exact current settings path/config for Cursor against its official docs
  - `sdks/editor-integrations/cursor.mdx:18`
- verify the exact current settings path/config for OpenClaw against its official docs, including whether it supports a custom OpenAI-compatible endpoint at all
  - `sdks/editor-integrations/openclaw.mdx:18`
- verify the exact current settings path/config for Zed against its official docs, including the configuration key names for a custom OpenAI-compatible provider
  - `sdks/editor-integrations/zed.mdx:18`

## DevRel/Infra (3)

- Datadog OTLP ingestion setup Confirm the exact collector or Datadog Agent configuration, the Datadog API key handling, and any attribute mapping required for spans and metrics to land correctly in Datadog.
  - `resources/observability/datadog.mdx:33`
- confirm whether `X-Request-ID` is attached to Sentry events as a tag or context field If it is not automatic, correlation may require matching on timestamp and route.
  - `resources/observability/sentry.mdx:29`
- confirm whether a first-party "export telemetry to S3" feature is planned The internal `BLOB_STORAGE_*` storage is not a customer-facing export today.
  - `resources/observability/s3.mdx:33`

## DevRel / Product (2)

- an explicit Anthropic-name to Nito-ID equivalence table for teams migrating from specific Anthropic models. This needs a curated mapping owned by an SME and must not be guessed
  - `sdks/anthropic-drop-in/mapping-notes.mdx:49`
- an explicit OpenAI-name to Nito-ID equivalence table (for example, which Nito catalog IDs teams should reach for when migrating from `gpt-4o` or `text-embedding-3-large`). This requires a curated mapping owned by an SME and must not be guessed
  - `sdks/openai-drop-in/model-mapping.mdx:69`

## UNSPECIFIED (6)

- ...` marker instead of asserting a value. If you are a security reviewer, treat any unmarked number as confirmed and any TODO-marked window as pending sign-off.
  - `privacy/data-retention/index.mdx:18`
- confirm the exact retention windows, if any, for idempotency-key billing dedup, governed debug capture, and tenant saved chats. These were unverifiable in code and must be confirmed by Backend/Legal before being documented as facts.
  - `privacy/zero-data-retention.mdx:35`
- governed debug-capture retention. The retention doc references "bounded retention" with no number, and no debug-capture subsystem or duration constant exists in code. Verify with Backend.
  - `privacy/known-limitations.mdx:66`
- idempotency-replay retention window. `Idempotency-Key` drives billing dedup, but no replay/retention duration was found in code. Verify with Backend (may live in SQL migrations not scanned).
  - `privacy/known-limitations.mdx:64`
- replace `MODEL_ID` with a confirmed streaming-capable model ID from `GET /v1/models`.
  - `resources/best-practices/latency-and-performance.mdx:21`
- tenant saved-chat retention. No server-side saved-chat store with a retention window exists in code; share links persist only an object key and a token hash with no TTL. Verify with Backend.
  - `privacy/known-limitations.mdx:68`

## Backend / Security (2)

- BYOK (customer-supplied provider keys) is not implemented in the gateway. Confirm whether it is on the roadmap and the intended shape before documenting any flow Track on the deferred list.
  - `cookbook/private-ai/confidential-enterprise-inference.mdx:36`
- governed/audited debug capture and its retention window are not confirmed in code. Confirm whether the subsystem exists, its retention window, and its access-audit model before documenting it Track on the deferred list.
  - `cookbook/private-ai/confidential-enterprise-inference.mdx:42`

## Finance and Backend (1)

- the master per-model price table (input and output cost per model) and the human-facing credit unit (what one credit is worth in currency). These numbers live in the billing system and provider catalogs and are not asserted in these docs.
  - `models/pricing.mdx:22`

