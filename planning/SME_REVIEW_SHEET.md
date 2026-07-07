# Nito Docs — SME Review Sheet

One row per open question. Click **Live page** to see it in context, then write your answer in **SME input**.

- Live site: https://nito.mintlify.site
- Repo: https://github.com/RumeelHussain/nito-ai-docs
- Total: **119 distinct items** (the production base URL is one item shared across 109 pages).

---

## 0. Production base URL — Product/Infra (HIGH PRIORITY)

**Q:** What is the canonical production base URL? Every code sample uses the placeholder `https://YOUR-NITO-BASE-URL`. One confirmed value updates all pages at once.
- Shared across **109 pages**. Sample: [link](https://nito.mintlify.site/api-reference/attestation/get-attestation) · [link](https://nito.mintlify.site/api-reference/attestation/verify-yourself) · [link](https://nito.mintlify.site/api-reference/chat-and-text/chat-completions) · [link](https://nito.mintlify.site/api-reference/chat-and-text/chat-fusion)
- **SME input:** ____________________________________________

---

## Product — 10 question(s)

**1. Agent SDK repository, install steps, and links**
- [Live page](https://nito.mintlify.site/resources/powered-by-z/for-developers-building-agents)
- `resources/powered-by-z/for-developers-building-agents.mdx:29`
- **SME input:** ____________________________________________

**2. Z engine specifics and the Nito-to-Z bridge**
- [Live page](https://nito.mintlify.site/resources/powered-by-z)
- `resources/powered-by-z/index.mdx:28`
- **SME input:** ____________________________________________

**3. Z engine specifics and the Nito-to-Z bridge for agents**
- [Live page](https://nito.mintlify.site/resources/powered-by-z/for-developers-building-agents)
- `resources/powered-by-z/for-developers-building-agents.mdx:23`
- **SME input:** ____________________________________________

**4. a concrete attestation-capable `MODEL_ID` for the `:tee` example Confirm against `GET /v1/models`.**
- [Live page](https://nito.mintlify.site/cookbook/private-ai/build-document-qa) · [Live page](https://nito.mintlify.site/cookbook/private-ai/drop-in-privacy-upgrade)
- `cookbook/private-ai/build-document-qa.mdx:76` , `cookbook/private-ai/drop-in-privacy-upgrade.mdx:49`
- **SME input:** ____________________________________________

**5. confirm `ORG_ID` and `KEY_ID` retrieval flow and any UI equivalent for teams that do not script rotation**
- [Live page](https://nito.mintlify.site/resources/best-practices/secure-api-key-handling)
- `resources/best-practices/secure-api-key-handling.mdx:43`
- **SME input:** ____________________________________________

**6. confirm recommended primary and fallback model pairings once canonical model IDs are published**
- [Live page](https://nito.mintlify.site/resources/best-practices/uptime-and-fallbacks)
- `resources/best-practices/uptime-and-fallbacks.mdx:28`
- **SME input:** ____________________________________________

**7. confirm the canonical location and link for the deferred-capabilities list (audio in, video in, audio generation, video generation) so this page can point readers to a single source.**
- [Live page](https://nito.mintlify.site/features/multimodal)
- `features/multimodal.mdx:43`
- **SME input:** ____________________________________________

**8. the formal deprecation and retirement policy, including advance-notice windows and timelines. The gateway tracks availability and activation and marks models unavailable on sync, but there is no codified deprecation schedule, sunset notice, or version-deprecation mechanism at this commit. Until this is published, treat the practices below as the guidance.**
- [Live page](https://nito.mintlify.site/models/deprecations-and-lifecycle)
- `models/deprecations-and-lifecycle.mdx:18`
- **SME input:** ____________________________________________

**9. whether audio/video generation is on the roadmap Tracked as a deferred capability.**
- [Live page](https://nito.mintlify.site/api-reference/multimodal/image-generation)
- `api-reference/multimodal/image-generation.mdx:90`
- **SME input:** ____________________________________________

**10. whether audio/video input is on the roadmap Tracked as a deferred capability.**
- [Live page](https://nito.mintlify.site/api-reference/multimodal/pdf-input)
- `api-reference/multimodal/pdf-input.mdx:70`
- **SME input:** ____________________________________________


## Product/Infra — 1 question(s)

**11. publish a status page / per-surface uptime Confirm the hosting approach, the surfaces to report on, the incident and maintenance workflow, and the subscription mechanism before any of the content below is presented as live.**
- [Live page](https://nito.mintlify.site/resources/status-page)
- `resources/status-page.mdx:8`
- **SME input:** ____________________________________________


## Product/Backend — 1 question(s)

**12. tenant saved-chat retention (30d/90d) is not present in current code; confirm roadmap**
- [Live page](https://nito.mintlify.site/privacy/data-retention/tenant-saved-chat)
- `privacy/data-retention/tenant-saved-chat.mdx:35`
- **SME input:** ____________________________________________


## Product/DevRel — 1 question(s)

**13. confirm whether outbound consumer webhooks are planned, and if so the event catalog, signing scheme, and delivery guarantees Until then, this page must not describe an outbound webhook feature as available.**
- [Live page](https://nito.mintlify.site/resources/observability/webhooks)
- `resources/observability/webhooks.mdx:30`
- **SME input:** ____________________________________________


## Product/Support — 1 question(s)

**14. confirm any gateway-side lookup or support flow that consumes a client-supplied `X-Request-ID`, so docs can tell users how to hand an ID to support**
- [Live page](https://nito.mintlify.site/resources/best-practices/privacy-safe-logging)
- `resources/best-practices/privacy-safe-logging.mdx:20`
- **SME input:** ____________________________________________


## Product/Legal — 1 question(s)

**15. confirm the relationship between the status page and any SLA Until an SLA exists, the status page is informational only.**
- [Live page](https://nito.mintlify.site/resources/status-page)
- `resources/status-page.mdx:27`
- **SME input:** ____________________________________________


## Backend — 19 question(s)

**16. confirm the exact fields and granularity returned by the org usage overview for a cost-monitoring dashboard**
- [Live page](https://nito.mintlify.site/resources/best-practices/cost-control)
- `resources/best-practices/cost-control.mdx:32`
- **SME input:** ____________________________________________

**17. confirm there is no idempotency response-replay or retention window anywhere below the server layer (for example in SQL migrations). The scanned Go shows only billing dedup and no 24h replay window; the source of truth flags the "24h replay" claim as unconfirmed**
- [Live page](https://nito.mintlify.site/features/idempotency)
- `features/idempotency.mdx:28`
- **SME input:** ____________________________________________

**18. confirm whether the 80-page, 10-second, and 300,000-character caps apply identically to the inline `file` content-part path and the `/api/v1/attachments/parse` endpoint, or whether they differ between the two.**
- [Live page](https://nito.mintlify.site/features/multimodal/pdf-input)
- `features/multimodal/pdf-input.mdx:92`
- **SME input:** ____________________________________________

**19. confirm whether the per-image 5 MiB, combined 50 MiB, 10-image, and 2048 px long-edge limits also bound images supplied as data URLs or base64 inside the `image_url` content part, or whether those numbers apply only to the multipart upload path.**
- [Live page](https://nito.mintlify.site/features/multimodal/image-input)
- `features/multimodal/image-input.mdx:68`
- **SME input:** ____________________________________________

**20. idempotency replay/retention window The mechanism exists for billing dedup; any specific replay-retention window is unconfirmed and may live in infrastructure or SQL migrations not covered by this scan.**
- [Live page](https://nito.mintlify.site/privacy/data-retention/idempotency-replay)
- `privacy/data-retention/idempotency-replay.mdx:32`
- **SME input:** ____________________________________________

**21. per-model embedding output dimensions**
- [Live page](https://nito.mintlify.site/api-reference/embeddings/vector-dimensions)
- `api-reference/embeddings/vector-dimensions.mdx:39`
- **SME input:** ____________________________________________

**22. publish per-model output dimensions for the default and other embedding models. This is not encoded in the gateway; the provider returns the vector length at runtime, so it must come from Backend.**
- [Live page](https://nito.mintlify.site/features/embeddings)
- `features/embeddings.mdx:96`
- **SME input:** ____________________________________________

**23. publish the OpenAPI spec at `docs/planning/openapi.yaml`**
- [Live page](https://nito.mintlify.site/api-reference/overview/openapi-spec)
- `api-reference/overview/openapi-spec.mdx:8`
- **SME input:** ____________________________________________

**24. the enumerated value sets for `mode`, `retrieval_mode`, `freshness`, `safesearch`, and the `location` object shape**
- [Live page](https://nito.mintlify.site/api-reference/server-tools/web-search)
- `api-reference/server-tools/web-search.mdx:60`
- **SME input:** ____________________________________________

**25. the exact `web_search` metadata field set on the response**
- [Live page](https://nito.mintlify.site/api-reference/server-tools/web-search)
- `api-reference/server-tools/web-search.mdx:89`
- **SME input:** ____________________________________________

**26. the exact accepted value sets and formats for `freshness`, `country`, `search_lang`, and `safesearch` (for example whether `freshness` uses Brave-style codes like `pd`/`pw`/`pm`/`py` or date ranges). Confirm against `internal/websearch/` before publishing literal values**
- [Live page](https://nito.mintlify.site/features/server-side-tools/web-search)
- `features/server-side-tools/web-search.mdx:68`
- **SME input:** ____________________________________________

**27. the exact field layout of the response-level `web_search` object (source URLs, titles, snippets, ordering). Document against the actual serialized shape before publishing**
- [Live page](https://nito.mintlify.site/features/server-side-tools/web-search)
- `features/server-side-tools/web-search.mdx:135`
- **SME input:** ____________________________________________

**28. the exact per-option field set**
- [Live page](https://nito.mintlify.site/api-reference/models-catalog/model-groups)
- `api-reference/models-catalog/model-groups.mdx:46`
- **SME input:** ____________________________________________

**29. the full field list inside `daily[]` and `models[]` entries (beyond `requests`, token counts, `customer_charge_nanos`, and the daily top model). Confirm against a live response**
- [Live page](https://nito.mintlify.site/cookbook/advanced/usage-dashboard)
- `cookbook/advanced/usage-dashboard.mdx:85`
- **SME input:** ____________________________________________

**30. the gateway's exact upstream retry set and schedule (`internal/inference/retry.go` is not fully enumerated)**
- [Live page](https://nito.mintlify.site/api-reference/errors-and-rate-limits/retry-behavior)
- `api-reference/errors-and-rate-limits/retry-behavior.mdx:34`
- **SME input:** ____________________________________________

**31. the per-model embedding output dimensions. The gateway does not encode a fixed dimension per model; the vector length is returned by the provider at runtime**
- [Live page](https://nito.mintlify.site/models/model-families)
- `models/model-families.mdx:28`
- **SME input:** ____________________________________________

**32. the response shape for code-interpreter output as relayed by the gateway (tool-result content parts, execution metadata, and billing treatment). Confirm against the xAI passthrough path before publishing specifics**
- [Live page](https://nito.mintlify.site/features/server-side-tools/code-interpreter)
- `features/server-side-tools/code-interpreter.mdx:56`
- **SME input:** ____________________________________________

**33. the response-level metadata shape for web fetch (fetched URL, content length, any per-page status). Confirm against the serialized response before publishing literal fields**
- [Live page](https://nito.mintlify.site/features/server-side-tools/web-fetch)
- `features/server-side-tools/web-fetch.mdx:57`
- **SME input:** ____________________________________________

**34. the values above are illustrative. Confirm a real catalog row, its `context_window`, and `pricing` shape**
- [Live page](https://nito.mintlify.site/api-reference/models-catalog/list-models)
- `api-reference/models-catalog/list-models.mdx:124`
- **SME input:** ____________________________________________


## Backend/Product — 5 question(s)

**35. a curated table of recommended embedding models with their providers and intended use**
- [Live page](https://nito.mintlify.site/api-reference/embeddings/embedding-models)
- `api-reference/embeddings/embedding-models.mdx:27`
- **SME input:** ____________________________________________

**36. confirm published production rate limits**
- [Live page](https://nito.mintlify.site/resources/rate-limits)
- `resources/rate-limits.mdx:16`
- **SME input:** ____________________________________________

**37. confirm published production rate limits and any per-deployment `Retry-After` sizing**
- [Live page](https://nito.mintlify.site/resources/rate-limits)
- `resources/rate-limits.mdx:65`
- **SME input:** ____________________________________________

**38. whether there is a supported way to pin a specific physical backend (a provider-scoped `provider/model` ID) from the public API, beyond the model ID plus privacy suffix. Confirm the intended public contract**
- [Live page](https://nito.mintlify.site/models/model-id-convention)
- `models/model-id-convention.mdx:62`
- **SME input:** ____________________________________________

**39. whether there is a supported way to pin a specific physical backend (for example a provider-scoped `provider/model` ID) from the public API, beyond the model ID and privacy mode. The routing logic chooses among candidates; confirm the intended public contract for explicit pinning**
- [Live page](https://nito.mintlify.site/models/backend-selection)
- `models/backend-selection.mdx:60`
- **SME input:** ____________________________________________


## Backend/Security — 1 question(s)

**40. debug capture mechanism and retention bound The concept (explicit opt-in, bounded retention) is described in the privacy standard; the implementing subsystem and the specific retention duration are not verifiable in the scanned code.**
- [Live page](https://nito.mintlify.site/privacy/data-retention/governed-debug-capture)
- `privacy/data-retention/governed-debug-capture.mdx:24`
- **SME input:** ____________________________________________


## Backend/Infra — 1 question(s)

**41. the exact published limits per deployment, if they differ from these defaults**
- [Live page](https://nito.mintlify.site/api-reference/errors-and-rate-limits/rate-limits)
- `api-reference/errors-and-rate-limits/rate-limits.mdx:31`
- **SME input:** ____________________________________________


## Infra/Backend — 1 question(s)

**42. the exact `OTEL_*` and `SENTRY_*` variable names, required values, and which exporters/protocols are wired. Confirm against the gateway's `.env` configuration**
- [Live page](https://nito.mintlify.site/cookbook/advanced/observability-pipeline)
- `cookbook/advanced/observability-pipeline.mdx:71`
- **SME input:** ____________________________________________


## Finance — 5 question(s)

**43. image output pricing basis and per-model rates**
- [Live page](https://nito.mintlify.site/resources/pricing)
- `resources/pricing.mdx:74`
- **SME input:** ____________________________________________

**44. master per-model price table and credit unit**
- [Live page](https://nito.mintlify.site/resources/pricing)
- `resources/pricing.mdx:16`
- **SME input:** ____________________________________________

**45. server-side tool unit prices**
- [Live page](https://nito.mintlify.site/api-reference/server-tools/tool-pricing) · [Live page](https://nito.mintlify.site/resources/pricing)
- `api-reference/server-tools/tool-pricing.mdx:21` , `resources/pricing.mdx:68`
- **SME input:** ____________________________________________

**46. server-side tool unit prices.**
- [Live page](https://nito.mintlify.site/models/pricing)
- `models/pricing.mdx:28`
- **SME input:** ____________________________________________

**47. the master per-model price table and the credit-to-currency value**
- [Live page](https://nito.mintlify.site/faq/models-and-pricing)
- `faq/models-and-pricing.mdx:20`
- **SME input:** ____________________________________________


## Finance/Backend — 4 question(s)

**48. a worked example response showing the exact `pricing` object field names and sample nanos values**
- [Live page](https://nito.mintlify.site/resources/pricing)
- `resources/pricing.mdx:45`
- **SME input:** ____________________________________________

**49. per-model pricing and context windows, and per-model embedding output dimensions, are not encoded on the model card and are not published here**
- [Live page](https://nito.mintlify.site/privacy/known-limitations)
- `privacy/known-limitations.mdx:76`
- **SME input:** ____________________________________________

**50. publish the per-pixel pricing (or the formula) and confirm how `n` and `size` combine into the billed amount, including any minimum charge. Pricing is not encoded in the gateway model card.**
- [Live page](https://nito.mintlify.site/features/multimodal/image-generation)
- `features/multimodal/image-generation.mdx:73`
- **SME input:** ____________________________________________

**51. the full `pricing` field set and unit definitions per model type**
- [Live page](https://nito.mintlify.site/api-reference/models-catalog/list-models)
- `api-reference/models-catalog/list-models.mdx:87`
- **SME input:** ____________________________________________


## Finance/Legal — 2 question(s)

**52. any specific privacy guarantee about payments (unlinkability of funding to requests, payer anonymity for on-chain top-ups, formal separation of payment identity from usage). State only what Legal/Finance can stand behind**
- [Live page](https://nito.mintlify.site/privacy/payment-privacy)
- `privacy/payment-privacy.mdx:46`
- **SME input:** ____________________________________________

**53. retention and visibility of payment records and on-chain transaction references, and how long funding history is kept**
- [Live page](https://nito.mintlify.site/privacy/payment-privacy)
- `privacy/payment-privacy.mdx:48`
- **SME input:** ____________________________________________


## Legal — 12 question(s)

**54. DPA text and execution process The full Data Processing Addendum, including the signing or execution flow (self-serve acceptance, countersignature, or a portal), is not yet available and must be authored and approved by Legal.**
- [Live page](https://nito.mintlify.site/privacy/dpa)
- `privacy/dpa.mdx:14`
- **SME input:** ____________________________________________

**55. DPA text, execution process, and publication, including alignment with the subprocessor list and any GDPR/CCPA role definitions**
- [Live page](https://nito.mintlify.site/resources/trust-center/dpa)
- `resources/trust-center/dpa.mdx:34`
- **SME input:** ____________________________________________

**56. authoritative subprocessor list and change-notification process, including the correct legal role for each named provider (xAI, OpenRouter, NEAR, Phala, fal, Stripe) and any others in the data path**
- [Live page](https://nito.mintlify.site/resources/trust-center/subprocessors)
- `resources/trust-center/subprocessors.mdx:30`
- **SME input:** ____________________________________________

**57. categories of data processed and categories of data subjects**
- [Live page](https://nito.mintlify.site/privacy/dpa)
- `privacy/dpa.mdx:22`
- **SME input:** ____________________________________________

**58. cookie categorization, consent requirements, third-party cookie disclosures, and the final cookie notice text**
- [Live page](https://nito.mintlify.site/resources/policies/cookies)
- `resources/policies/cookies.mdx:30`
- **SME input:** ____________________________________________

**59. full Acceptable Use Policy text and publication, including prohibited-use categories, enforcement process, and alignment with the Terms of Service**
- [Live page](https://nito.mintlify.site/resources/policies/acceptable-use-policy)
- `resources/policies/acceptable-use-policy.mdx:26`
- **SME input:** ____________________________________________

**60. full Privacy Policy text and its publication at `/privacy`, including data-subject rights processes and any GDPR/CCPA role definitions**
- [Live page](https://nito.mintlify.site/resources/policies/privacy-policy)
- `resources/policies/privacy-policy.mdx:30`
- **SME input:** ____________________________________________

**61. full Terms of Service text and its publication at `/terms`, including alignment with the Acceptable Use Policy and Privacy Policy**
- [Live page](https://nito.mintlify.site/resources/policies/terms-of-service)
- `resources/policies/terms-of-service.mdx:33`
- **SME input:** ____________________________________________

**62. international transfer mechanisms, to the extent any are offered**
- [Live page](https://nito.mintlify.site/privacy/dpa)
- `privacy/dpa.mdx:28`
- **SME input:** ____________________________________________

**63. roles of the parties (controller and processor designations) and the scope of processing**
- [Live page](https://nito.mintlify.site/privacy/dpa)
- `privacy/dpa.mdx:20`
- **SME input:** ____________________________________________

**64. subprocessor list and the process for notifying and objecting to subprocessor changes. This intersects the providers behind each privacy mode described in [Provider-side enforcement](/privacy/provider-side-enforcement)**
- [Live page](https://nito.mintlify.site/privacy/dpa)
- `privacy/dpa.mdx:24`
- **SME input:** ____________________________________________

**65. the execution process: how a customer requests, reviews, and signs the DPA, and where the signed copy is stored**
- [Live page](https://nito.mintlify.site/privacy/dpa)
- `privacy/dpa.mdx:30`
- **SME input:** ____________________________________________


## Legal/Security — 8 question(s)

**66. GDPR and CCPA roles and processes (controller vs processor, data-subject request handling, lawful basis), to the extent any are claimed**
- [Live page](https://nito.mintlify.site/privacy/compliance-posture)
- `privacy/compliance-posture.mdx:36`
- **SME input:** ____________________________________________

**67. HIPAA posture. Whether Nito is "HIPAA-adjacent," supports a BAA, or handles PHI in any defined way is not established in code and is not asserted. Do not imply HIPAA alignment**
- [Live page](https://nito.mintlify.site/privacy/compliance-posture)
- `privacy/compliance-posture.mdx:28`
- **SME input:** ____________________________________________

**68. SLA terms, including the availability target, measurement method, support-response commitments, and remedies. No uptime number may be published until this is authored**
- [Live page](https://nito.mintlify.site/resources/trust-center/sla)
- `resources/trust-center/sla.mdx:28`
- **SME input:** ____________________________________________

**69. SOC 2. There is no SOC 2 report, audit status, or trust-services-criteria claim in the codebase. Do not state or imply SOC 2 Type I or Type II**
- [Live page](https://nito.mintlify.site/privacy/compliance-posture)
- `privacy/compliance-posture.mdx:30`
- **SME input:** ____________________________________________

**70. any SLA or uptime commitment. None is established in code and none is asserted here**
- [Live page](https://nito.mintlify.site/privacy/compliance-posture)
- `privacy/compliance-posture.mdx:38`
- **SME input:** ____________________________________________

**71. security measures, incident notification timelines, and audit rights**
- [Live page](https://nito.mintlify.site/privacy/dpa)
- `privacy/dpa.mdx:26`
- **SME input:** ____________________________________________

**72. subprocessor list and data-flow documentation, including the providers behind each privacy mode, framed for a compliance audience**
- [Live page](https://nito.mintlify.site/privacy/compliance-posture)
- `privacy/compliance-posture.mdx:34`
- **SME input:** ____________________________________________

**73. the exact contractual data-processing terms behind each provider's no-retention setting, beyond the per-request technical flag enforced in code**
- [Live page](https://nito.mintlify.site/privacy/provider-side-enforcement)
- `privacy/provider-side-enforcement.mdx:64`
- **SME input:** ____________________________________________


## Security — 4 question(s)

**74. advisory publication process, severity methodology, notification channel, and any historical advisories. No advisory content or feed may be published until this is defined**
- [Live page](https://nito.mintlify.site/resources/trust-center/security-advisories)
- `resources/trust-center/security-advisories.mdx:31`
- **SME input:** ____________________________________________

**75. exact E2E-TEE encryption handshake and header value construction**
- [Live page](https://nito.mintlify.site/cookbook/private-ai/end-to-end-tee-call)
- `cookbook/private-ai/end-to-end-tee-call.mdx:31`
- **SME input:** ____________________________________________

**76. reporting contact and encryption key, scope definition, response-time commitments, and final safe-harbor terms**
- [Live page](https://nito.mintlify.site/resources/trust-center/responsible-disclosure)
- `resources/trust-center/responsible-disclosure.mdx:26`
- **SME input:** ____________________________________________

**77. the exact invocation and any build step for the reference verifier (for example a `make` target), confirmed against the current repo**
- [Live page](https://nito.mintlify.site/privacy/tee-attestation/verify-yourself-recipe)
- `privacy/tee-attestation/verify-yourself-recipe.mdx:103`
- **SME input:** ____________________________________________


## Security / Product — 1 question(s)

**78. confirmation of which public models map to NEAR versus Phala backends, and whether any model exposes both. The mapping is runtime-discovered, so there is no static list to publish here**
- [Live page](https://nito.mintlify.site/privacy/tee-attestation/per-provider-attestation)
- `privacy/tee-attestation/per-provider-attestation.mdx:58`
- **SME input:** ____________________________________________


## Security/Backend — 1 question(s)

**79. confirm the exact construction of each `X-Z-TEE-*` header value (key encodings, supported signing algorithms, encryption version string, nonce/timestamp format) and provide a complete, runnable end-to-end example with real values**
- [Live page](https://nito.mintlify.site/privacy/per-call-privacy-modes)
- `privacy/per-call-privacy-modes.mdx:111`
- **SME input:** ____________________________________________


## DevRel — 23 question(s)

**80. GitHub, changelog, and announcement channel links**
- [Live page](https://nito.mintlify.site/resources/community)
- `resources/community.mdx:24`
- **SME input:** ____________________________________________

**81. community channel links, including support forum and Discord**
- [Live page](https://nito.mintlify.site/resources/community)
- `resources/community.mdx:12`
- **SME input:** ____________________________________________

**82. community showcase and project-sharing links**
- [Live page](https://nito.mintlify.site/resources/community)
- `resources/community.mdx:18`
- **SME input:** ____________________________________________

**83. confirm Langfuse integration path Determine whether a first-party Langfuse export is planned, and if so what it would send.**
- [Live page](https://nito.mintlify.site/resources/observability/langfuse)
- `resources/observability/langfuse.mdx:37`
- **SME input:** ____________________________________________

**84. publish Postman/Bruno collection files**
- [Live page](https://nito.mintlify.site/sdks/postman-bruno)
- `sdks/postman-bruno.mdx:10`
- **SME input:** ____________________________________________

**85. verify the current Discord API/library setup against its official docs**
- [Live page](https://nito.mintlify.site/cookbook/bots/discord-bot)
- `cookbook/bots/discord-bot.mdx:8`
- **SME input:** ____________________________________________

**86. verify the current OpenClaw gateway/provider setup against its official docs**
- [Live page](https://nito.mintlify.site/cookbook/bots/openclaw-gateway)
- `cookbook/bots/openclaw-gateway.mdx:8`
- **SME input:** ____________________________________________

**87. verify the current Slack app/API/SDK setup against its official docs**
- [Live page](https://nito.mintlify.site/cookbook/bots/slack-assistant)
- `cookbook/bots/slack-assistant.mdx:8`
- **SME input:** ____________________________________________

**88. verify the current Telegram Bot API/library setup against its official docs**
- [Live page](https://nito.mintlify.site/cookbook/bots/telegram-bot)
- `cookbook/bots/telegram-bot.mdx:8`
- **SME input:** ____________________________________________

**89. verify the current WhatsApp messaging API/provider setup against its official docs**
- [Live page](https://nito.mintlify.site/cookbook/bots/whatsapp-assistant)
- `cookbook/bots/whatsapp-assistant.mdx:8`
- **SME input:** ____________________________________________

**90. verify the exact current configuration for CrewAI against its official docs**
- [Live page](https://nito.mintlify.site/sdks/framework-integrations/crewai)
- `sdks/framework-integrations/crewai.mdx:37`
- **SME input:** ____________________________________________

**91. verify the exact current configuration for LangChain and LangGraph against its official docs**
- [Live page](https://nito.mintlify.site/sdks/framework-integrations/langchain-langgraph)
- `sdks/framework-integrations/langchain-langgraph.mdx:35`
- **SME input:** ____________________________________________

**92. verify the exact current configuration for LiteLLM (SDK and proxy) against its official docs**
- [Live page](https://nito.mintlify.site/sdks/framework-integrations/litellm)
- `sdks/framework-integrations/litellm.mdx:35`
- **SME input:** ____________________________________________

**93. verify the exact current configuration for LlamaIndex (LLM and embeddings) against its official docs**
- [Live page](https://nito.mintlify.site/sdks/framework-integrations/llamaindex)
- `sdks/framework-integrations/llamaindex.mdx:35`
- **SME input:** ____________________________________________

**94. verify the exact current configuration for Mastra against its official docs**
- [Live page](https://nito.mintlify.site/sdks/framework-integrations/mastra)
- `sdks/framework-integrations/mastra.mdx:36`
- **SME input:** ____________________________________________

**95. verify the exact current configuration for Pydantic AI against its official docs**
- [Live page](https://nito.mintlify.site/sdks/framework-integrations/pydantic-ai)
- `sdks/framework-integrations/pydantic-ai.mdx:38`
- **SME input:** ____________________________________________

**96. verify the exact current configuration for the Vercel AI SDK against its official docs**
- [Live page](https://nito.mintlify.site/sdks/framework-integrations/vercel-ai-sdk)
- `sdks/framework-integrations/vercel-ai-sdk.mdx:39`
- **SME input:** ____________________________________________

**97. verify the exact current settings path/config for Claude Code against its official docs, including whether the running version supports an OpenAI-compatible endpoint at all**
- [Live page](https://nito.mintlify.site/sdks/editor-integrations/claude-code)
- `sdks/editor-integrations/claude-code.mdx:18`
- **SME input:** ____________________________________________

**98. verify the exact current settings path/config for Codex against its official docs, including whether the running version targets Chat Completions or the Responses API**
- [Live page](https://nito.mintlify.site/sdks/editor-integrations/codex)
- `sdks/editor-integrations/codex.mdx:18`
- **SME input:** ____________________________________________

**99. verify the exact current settings path/config for Continue.dev against its official docs, including the configuration key names for an OpenAI-compatible model entry**
- [Live page](https://nito.mintlify.site/sdks/editor-integrations/continue-dev)
- `sdks/editor-integrations/continue-dev.mdx:18`
- **SME input:** ____________________________________________

**100. verify the exact current settings path/config for Cursor against its official docs**
- [Live page](https://nito.mintlify.site/sdks/editor-integrations/cursor)
- `sdks/editor-integrations/cursor.mdx:18`
- **SME input:** ____________________________________________

**101. verify the exact current settings path/config for OpenClaw against its official docs, including whether it supports a custom OpenAI-compatible endpoint at all**
- [Live page](https://nito.mintlify.site/sdks/editor-integrations/openclaw)
- `sdks/editor-integrations/openclaw.mdx:18`
- **SME input:** ____________________________________________

**102. verify the exact current settings path/config for Zed against its official docs, including the configuration key names for a custom OpenAI-compatible provider**
- [Live page](https://nito.mintlify.site/sdks/editor-integrations/zed)
- `sdks/editor-integrations/zed.mdx:18`
- **SME input:** ____________________________________________


## DevRel/Infra — 3 question(s)

**103. Datadog OTLP ingestion setup Confirm the exact collector or Datadog Agent configuration, the Datadog API key handling, and any attribute mapping required for spans and metrics to land correctly in Datadog.**
- [Live page](https://nito.mintlify.site/resources/observability/datadog)
- `resources/observability/datadog.mdx:33`
- **SME input:** ____________________________________________

**104. confirm whether `X-Request-ID` is attached to Sentry events as a tag or context field If it is not automatic, correlation may require matching on timestamp and route.**
- [Live page](https://nito.mintlify.site/resources/observability/sentry)
- `resources/observability/sentry.mdx:29`
- **SME input:** ____________________________________________

**105. confirm whether a first-party "export telemetry to S3" feature is planned The internal `BLOB_STORAGE_*` storage is not a customer-facing export today.**
- [Live page](https://nito.mintlify.site/resources/observability/s3)
- `resources/observability/s3.mdx:33`
- **SME input:** ____________________________________________


## DevRel / Product — 2 question(s)

**106. an explicit Anthropic-name to Nito-ID equivalence table for teams migrating from specific Anthropic models. This needs a curated mapping owned by an SME and must not be guessed**
- [Live page](https://nito.mintlify.site/sdks/anthropic-drop-in/mapping-notes)
- `sdks/anthropic-drop-in/mapping-notes.mdx:49`
- **SME input:** ____________________________________________

**107. an explicit OpenAI-name to Nito-ID equivalence table (for example, which Nito catalog IDs teams should reach for when migrating from `gpt-4o` or `text-embedding-3-large`). This requires a curated mapping owned by an SME and must not be guessed**
- [Live page](https://nito.mintlify.site/sdks/openai-drop-in/model-mapping)
- `sdks/openai-drop-in/model-mapping.mdx:69`
- **SME input:** ____________________________________________


## UNSPECIFIED — 6 question(s)

**108. ...` marker instead of asserting a value. If you are a security reviewer, treat any unmarked number as confirmed and any TODO-marked window as pending sign-off.**
- [Live page](https://nito.mintlify.site/privacy/data-retention)
- `privacy/data-retention/index.mdx:18`
- **SME input:** ____________________________________________

**109. confirm the exact retention windows, if any, for idempotency-key billing dedup, governed debug capture, and tenant saved chats. These were unverifiable in code and must be confirmed by Backend/Legal before being documented as facts.**
- [Live page](https://nito.mintlify.site/privacy/zero-data-retention)
- `privacy/zero-data-retention.mdx:35`
- **SME input:** ____________________________________________

**110. governed debug-capture retention. The retention doc references "bounded retention" with no number, and no debug-capture subsystem or duration constant exists in code. Verify with Backend.**
- [Live page](https://nito.mintlify.site/privacy/known-limitations)
- `privacy/known-limitations.mdx:66`
- **SME input:** ____________________________________________

**111. idempotency-replay retention window. `Idempotency-Key` drives billing dedup, but no replay/retention duration was found in code. Verify with Backend (may live in SQL migrations not scanned).**
- [Live page](https://nito.mintlify.site/privacy/known-limitations)
- `privacy/known-limitations.mdx:64`
- **SME input:** ____________________________________________

**112. replace `MODEL_ID` with a confirmed streaming-capable model ID from `GET /v1/models`.**
- [Live page](https://nito.mintlify.site/resources/best-practices/latency-and-performance)
- `resources/best-practices/latency-and-performance.mdx:21`
- **SME input:** ____________________________________________

**113. tenant saved-chat retention. No server-side saved-chat store with a retention window exists in code; share links persist only an object key and a token hash with no TTL. Verify with Backend.**
- [Live page](https://nito.mintlify.site/privacy/known-limitations)
- `privacy/known-limitations.mdx:68`
- **SME input:** ____________________________________________


## Backend / Security — 2 question(s)

**114. BYOK (customer-supplied provider keys) is not implemented in the gateway. Confirm whether it is on the roadmap and the intended shape before documenting any flow Track on the deferred list.**
- [Live page](https://nito.mintlify.site/cookbook/private-ai/confidential-enterprise-inference)
- `cookbook/private-ai/confidential-enterprise-inference.mdx:36`
- **SME input:** ____________________________________________

**115. governed/audited debug capture and its retention window are not confirmed in code. Confirm whether the subsystem exists, its retention window, and its access-audit model before documenting it Track on the deferred list.**
- [Live page](https://nito.mintlify.site/cookbook/private-ai/confidential-enterprise-inference)
- `cookbook/private-ai/confidential-enterprise-inference.mdx:42`
- **SME input:** ____________________________________________


## Finance and Backend — 1 question(s)

**116. the master per-model price table (input and output cost per model) and the human-facing credit unit (what one credit is worth in currency). These numbers live in the billing system and provider catalogs and are not asserted in these docs.**
- [Live page](https://nito.mintlify.site/models/pricing)
- `models/pricing.mdx:22`
- **SME input:** ____________________________________________


