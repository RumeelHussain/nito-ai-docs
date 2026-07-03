# Nito Documentation

Developer documentation for **Nito**, a private multi-LLM inference gateway (publicly _Z Inference_). Every page is generated from, and cited against, the `protocol-z/inference-gateway` codebase, so the docs reflect what the product actually does rather than what it aspires to do.

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
| --- | --- | --- |
| Get Started | `get-started/` | Intro, mental model, quickstarts, first request, streaming, usage |
| Models | `models/` | Catalog, model IDs, privacy tiers, providers, routing, pricing |
| Features | `features/` | Chat, streaming, tools, reasoning, multimodal, embeddings, caching, fusion |
| Privacy | `privacy/` | Tiers, TEE attestation, retention, threat model, compliance |
| SDKs | `sdks/` | OpenAI/Anthropic drop-ins, framework and editor integrations |
| API Reference | `api-reference/` | Endpoint reference (inference surface; account/billing pending) |
| Cookbook | `cookbook/` | Runnable recipes against the raw HTTP API |
| Resources | `resources/` | Pricing, rate limits, errors, best practices, trust center, FAQ |