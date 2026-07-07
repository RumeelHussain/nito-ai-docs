# Nito Docs — Non-SME TODO List

TODO-style markers that are **not** SME factual questions. These go to Design or the docs build, or are placeholders the reader fills in (like the base URL and example model IDs). The SME questions are in `SME_REVIEW_SHEET.md`.

- Live site: https://nito.mintlify.site
- Repo: https://github.com/RumeelHussain/nito-ai-docs
- Total actionable non-SME markers: **43**

> Note: most 'Example placeholders' resolve automatically once the production base URL and canonical model IDs are confirmed by SMEs. They are listed for completeness, not separate work.

---

## Design — diagrams (draw the visual) — 4

**1.** > **Diagram placeholder (request lifecycle).** Show: client request to the gateway; authentication and rate-limit check; model and privacy-mode resolution fr...
- [Live page](https://nito.mintlify.site/get-started/how-nito-works) — `get-started/how-nito-works.mdx:26`

**2.** > **Diagram placeholder (privacy architecture).** Show: client request entering the gateway; the auth hop resolving the API key to an organization; the model...
- [Live page](https://nito.mintlify.site/privacy/privacy-architecture) — `privacy/privacy-architecture.mdx:8`

**3.** > **Diagram placeholder (attestation verification).** Show: client generates a fresh nonce; client calls `GET /api/v1/tee/attestation?model=...&nonce=...`; g...
- [Live page](https://nito.mintlify.site/privacy/tee-attestation) — `privacy/tee-attestation/index.mdx:18`

**4.** > **Diagram placeholder (privacy tier flow).** Show the four tiers as a ladder from T1 Anonymous to T4 Encrypted (E2E TEE): for each rung, the named mode (`a...
- [Live page](https://nito.mintlify.site/privacy/tiers) — `privacy/tiers/index.mdx:41`


## Docs build — interactive components — 1

**5.** > **Interactive catalog widget:** the docs site will host a live, filterable catalog table. That component is a TODO for the docs build; until then, query `G...
- [Live page](https://nito.mintlify.site/models/model-catalog) — `models/model-catalog.mdx:65`


## Example placeholders (reader fills in) — 30

**6.** `# TODO: confirm canonical base URL` inside example code (resolves with the base-URL SME answer).
- Appears in **13 code samples**: [page](https://nito.mintlify.site/cookbook/advanced/cost-monitoring-dashboard) · [page](https://nito.mintlify.site/cookbook/advanced/fallback-router) · [page](https://nito.mintlify.site/cookbook/advanced/model-comparison-tool) · [page](https://nito.mintlify.site/cookbook/advanced/multimodal-app) · [page](https://nito.mintlify.site/cookbook/advanced/observability-pipeline) · [page](https://nito.mintlify.site/cookbook/advanced/privacy-first-workflow) …

**7.** # TODO: wire this to your notifier (email, Slack, PagerDuty, etc.)
- [Live page](https://nito.mintlify.site/cookbook/advanced/cost-monitoring-dashboard) — `cookbook/advanced/cost-monitoring-dashboard.mdx:70`

**8.** CHAT_MODEL = "MODEL_ID:tee" # TODO: attestation-capable model, or a private-tier model
- [Live page](https://nito.mintlify.site/cookbook/private-ai/build-document-qa) — `cookbook/private-ai/build-document-qa.mdx:56`

**9.** MODEL = "MODEL_ID:tee" # TODO: attestation-capable model; or a private-tier model
- [Live page](https://nito.mintlify.site/cookbook/private-ai/build-private-chat-app) — `cookbook/private-ai/build-private-chat-app.mdx:25`

**10.** EMBED_MODEL = "EMBED_MODEL_ID:tee" # TODO: attestation-capable or private-tier embedding model
- [Live page](https://nito.mintlify.site/cookbook/private-ai/build-private-rag-bot) — `cookbook/private-ai/build-private-rag-bot.mdx:24`

**11.** CHAT_MODEL = "CHAT_MODEL_ID:tee" # TODO: attestation-capable or private-tier chat model
- [Live page](https://nito.mintlify.site/cookbook/private-ai/build-private-rag-bot) — `cookbook/private-ai/build-private-rag-bot.mdx:51`

**12.** -H "X-Z-TEE-E2EE: <TODO>" \
- [Live page](https://nito.mintlify.site/cookbook/private-ai/end-to-end-tee-call) — `cookbook/private-ai/end-to-end-tee-call.mdx:43`

**13.** -H "X-Z-TEE-Client-Pub-Key: <TODO client public key>" \
- [Live page](https://nito.mintlify.site/cookbook/private-ai/end-to-end-tee-call) — `cookbook/private-ai/end-to-end-tee-call.mdx:44`

**14.** -H "X-Z-TEE-Model-Pub-Key: <TODO model public key from attestation>" \
- [Live page](https://nito.mintlify.site/cookbook/private-ai/end-to-end-tee-call) — `cookbook/private-ai/end-to-end-tee-call.mdx:45`

**15.** -H "X-Z-TEE-Signing-Algo: <TODO>" \
- [Live page](https://nito.mintlify.site/cookbook/private-ai/end-to-end-tee-call) — `cookbook/private-ai/end-to-end-tee-call.mdx:46`

**16.** -H "X-Z-TEE-Encryption-Version: <TODO>" \
- [Live page](https://nito.mintlify.site/cookbook/private-ai/end-to-end-tee-call) — `cookbook/private-ai/end-to-end-tee-call.mdx:47`

**17.** -H "X-Z-TEE-E2EE-Nonce: <TODO fresh nonce>" \
- [Live page](https://nito.mintlify.site/cookbook/private-ai/end-to-end-tee-call) — `cookbook/private-ai/end-to-end-tee-call.mdx:48`

**18.** -H "X-Z-TEE-E2EE-Timestamp: <TODO>" \
- [Live page](https://nito.mintlify.site/cookbook/private-ai/end-to-end-tee-call) — `cookbook/private-ai/end-to-end-tee-call.mdx:49`

**19.** MODEL = "MODEL_ID:tee" # TODO: attestation-capable model, or a private-tier model
- [Live page](https://nito.mintlify.site/cookbook/private-ai/structured-data-extraction) — `cookbook/private-ai/structured-data-extraction.mdx:22`

**20.** MODEL = "MODEL_ID:tee" # TODO: confirm an attestation-capable model
- [Live page](https://nito.mintlify.site/cookbook/private-ai/tee-attested-call-verify-nonce) — `cookbook/private-ai/tee-attested-call-verify-nonce.mdx:62`

**21.** # TODO (your responsibility, NOT done above):
- [Live page](https://nito.mintlify.site/cookbook/private-ai/tee-attested-call-verify-nonce) — `cookbook/private-ai/tee-attested-call-verify-nonce.mdx:91`

**22.** MODEL = "MODEL_ID:tee" # TODO: confirm an attestation-capable model
- [Live page](https://nito.mintlify.site/privacy/tee-attestation/verify-yourself-recipe) — `privacy/tee-attestation/verify-yourself-recipe.mdx:53`

**23.** # TODO (your responsibility, NOT done above):
- [Live page](https://nito.mintlify.site/privacy/tee-attestation/verify-yourself-recipe) — `privacy/tee-attestation/verify-yourself-recipe.mdx:85`


## Reader config (wire up your own tooling) — 1

**24.** > **TODO (config):** shipping the client-side records to Datadog, Langfuse, an S3 bucket, or any other sink. These destinations are not part of the gateway; ...
- [Live page](https://nito.mintlify.site/cookbook/advanced/observability-pipeline) — `cookbook/advanced/observability-pipeline.mdx:77`


## Other — 7

**25.** **What is buildable today:** attested inference on the `:tee` tier, with attestation you can fetch and verify. **What is not available yet:** BYOK and audite...
- [Live page](https://nito.mintlify.site/cookbook/private-ai/confidential-enterprise-inference) — `cookbook/private-ai/confidential-enterprise-inference.mdx:8`

**26.** This outline is a placeholder so the structure is ready when Legal provides the text. Every item below is a TODO owned by Legal; none is asserted as a curren...
- [Live page](https://nito.mintlify.site/privacy/dpa) — `privacy/dpa.mdx:18`

**27.** Observability for Nito (publicly Z Inference) has two halves: what the gateway emits server-side, and what you capture client-side from every response. This ...
- [Live page](https://nito.mintlify.site/resources/observability) — `resources/observability/index.mdx:6`

**28.** The cookie disclosures and any consent terms are owned by Legal and are not authored as policy in the codebase these docs are built from. The engineering fac...
- [Live page](https://nito.mintlify.site/resources/policies/cookies) — `resources/policies/cookies.mdx:9`

**29.** This is the authoritative reference for how Nito prices inference. It covers the billing unit, every rate that can apply to a call, and where to read the liv...
- [Live page](https://nito.mintlify.site/resources/pricing) — `resources/pricing.mdx:6`

**30.** description: A public status page for Nito is not published yet. This page describes what it will show and marks the work as a TODO owned by Product and Infra.
- [Live page](https://nito.mintlify.site/resources/status-page) — `resources/status-page.mdx:3`

**31.** Most artifacts linked below are owned by Legal and Security and are not yet published. Where a document is not final, the page for it is a stub that describe...
- [Live page](https://nito.mintlify.site/resources/trust-center) — `resources/trust-center/index.mdx:9`


