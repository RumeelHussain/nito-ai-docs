# DOCS_GAPS.md

Every fact the docs will need that could NOT be verified in code or an OpenAPI file at commit `6ada3ccf`. Each row: where it surfaces, the missing fact, likely owner, and why it could not be verified. Phase 2 appends a row per inline `TODO (needs SME)` marker.

| Page / area | Missing fact | Likely owner | Why unverified |
|---|---|---|---|
| Resources 8.1 / Models Pricing 2.10 / every model card | Per-model input & output pricing | Finance / Backend | Not stored on the model card; joined from `billing` package and live Stripe/OpenRouter catalog at response time. No static numbers in repo. |
| Models 2.x / API 5.3 | Per-model context window | Backend | Not on `SupportedModel`; joined from billing config; NEAR/Phala upstream `context_length` not persisted. |
| API 5.3 / Features Embeddings | Embedding output dimensions per model | Backend | Gateway only stores default model native ID + a 1–3072 request bound; actual vector length is returned by the provider at runtime. |
| Privacy: Data retention (idempotency replay 24h) | The 24h idempotency replay/retention window | Backend | `Idempotency-Key` drives billing dedup only; no 24h window in scanned Go. May be in SQL migrations. |
| Privacy: Governed debug capture (7d) | The 7-day debug-capture retention bound | Backend / Security | Doc says "bounded retention", no number; no debug-capture subsystem in code. |
| Privacy: Tenant saved chat (30d/90d) | Server-side saved-chat retention 30d default / 90d cap | Backend / Product | No server-side saved-chat store with TTL; chat history is client-side IndexedDB only. |
| Privacy: Three-distinct-keys | Whether the 3 secrets are independent | Security | By default 2 of 3 are HMAC-derived from `SESSION_SECRET`; independent only on explicit override. |
| Privacy: Compliance posture | HIPAA-adjacent / SOC 2 / EU residency status | Legal / Security | Always-TODO; no compliance assertions in code. |
| Privacy: DPA / Trust Center | DPA terms, subprocessor list, SLA | Legal | Not in repo. |
| API 5.1 Base URL | Canonical production base URL(s) | Product / Infra | Code default is `http://127.0.0.1:8080`; no production URL in repo. |
| API 5.17 / Resources 8.2 | Exact retryable status set & backoff schedule | Backend | `internal/inference/retry.go` not fully enumerated; only circuit-breaker config confirmed (threshold 3, cooldown 30s). |
| Resources 8.2 Rate limits | Whether documented limits are public/final | Backend / Product | Policy defaults in `internal/ratelimit/policy.go` are overridable per deployment via `RATE_LIMIT_POLICIES_FILE`. |
| Server tools 5.8 | Tool pricing (web search/fetch units) | Finance | Brave billed as pseudo-models; unit prices live in billing/Stripe config, not asserted. |
| API Reference (all) | Machine-readable request/response contracts | Backend | No `openapi.yaml` present; all schemas inferred from Go structs. Re-run Phase 2 when the spec lands. |

## Roadmap-or-cut decisions (drive stub vs delete; Product)
- Responses API (`/v1/responses`), Anthropic Messages (`/v1/messages`, `count_tokens`), Files (`/v1/files`) — currently 404. Stub or remove from outline?
- Audio input, video input, audio generation, video generation — absent. Roadmap or cut?
- Generation metadata (`/api/v1/generation`), outbound webhooks, BYOK, guardrails, structured outputs, message transforms, response caching, datetime tool — absent/not exposed. Stub or cut?
- `/api/v1/models/{id}/endpoints`, `/api/v1/providers` — absent. Cut or build?
