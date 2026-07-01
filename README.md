# Nito Documentation

Developer documentation for **Nito**, a private multi-LLM inference gateway (publicly *Z Inference*). Every page is generated from, and cited against, the `protocol-z/inference-gateway` codebase, so the docs reflect what the product actually does rather than what it aspires to do.

This repository is laid out as a [Mintlify](https://mintlify.com) site: `docs.json` at the root declares the navigation, and each page is an `.mdx` file.

## Preview locally

```bash
npm i -g mintlify   # one time
mintlify dev        # from the repo root
```

Then open the local URL Mintlify prints. Because `docs.json` is at the repository root, no extra configuration is needed.

## Structure

Eight tabs, 203 pages:

| Tab | Folder | What it covers |
|---|---|---|
| Get Started | `get-started/` | Intro, mental model, quickstarts, first request, streaming, usage |
| Models | `models/` | Catalog, model IDs, privacy tiers, providers, routing, pricing |
| Features | `features/` | Chat, streaming, tools, reasoning, multimodal, embeddings, caching, fusion |
| Privacy | `privacy/` | Tiers, TEE attestation, retention, threat model, compliance |
| SDKs | `sdks/` | OpenAI/Anthropic drop-ins, framework and editor integrations |
| API Reference | `api-reference/` | Endpoint reference (inference surface; account/billing pending) |
| Cookbook | `cookbook/` | Runnable recipes against the raw HTTP API |
| Resources | `resources/` | Pricing, rate limits, errors, best practices, trust center, FAQ |

## Planning and source material

The `planning/` folder holds the material behind the docs and is useful for review:

- **`Docs Outline Handoff.md`** the source outline.
- **`Nito_Docs_Section_Descriptions.md`** the per-page briefs.
- **`Z Protocol, Z Inference (Nito), and Core Explained.md`** background explainer.
- **`Nito_Docs_ClaudeCode_Prompt.md`** the generation prompt and house rules.
- **`DOCS_SOURCE_OF_TRUTH.md`** the code extraction every page is built on, with the analyzed commit hash and `file:symbol` citations.
- **`DOCS_GAPS.md`** every fact that could not be verified in code, with the likely owner.
- **`DOCS_DEFERRED.md`** outline nodes intentionally not written yet (separate repos, or endpoints that return 404).

## Conventions

- **Code is the source of truth.** Anything not verifiable in the gateway code or an OpenAPI spec is marked `> **TODO (needs SME):** ...` rather than stated as fact. Each page ends with an HTML `<!-- source: ... -->` comment listing the exact files and symbols it derives from.
- **House style.** No em dashes. Privacy language stays "attested" or "verifiable," never "trustless." The only reference to the underlying engine is the "Powered by Z" section.
- **Placeholders.** The production base URL is not yet finalized, so examples use `https://YOUR-NITO-BASE-URL`. Replace it once the canonical URL is confirmed.

## Status

- **Complete:** Get Started, Models, Features, Privacy, SDKs, Cookbook, Resources, and API Reference for the inference surface.
- **Pending the OpenAPI/Swagger file:** the account and billing half of the API Reference, plus reconciling the inference reference against the spec.
- **Pending team confirmation (tracked in `planning/DOCS_GAPS.md`):** production base URL, several retention windows, per-model pricing, and the compliance and legal artifacts.
