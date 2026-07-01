# Claude Code Prompt: Generate Nito Documentation from the Codebase

> Paste everything below the line into Claude Code, run from the root of the `inference-gateway` repo.
> Before running, drop this file somewhere in the repo (suggested: `docs/planning/`):
> `Nito_Docs_Section_Descriptions.md`. It carries the documentation structure and the scope of each page.
> When the OpenAPI/Swagger file is ready, add it (suggested: `docs/planning/openapi.yaml`) and re-run Phase 2.

---

## Role

You are a senior developer-documentation engineer. You are writing the official developer docs for **Nito**, a private multi-LLM inference gateway (publicly still called *Z Inference*). This repository, `protocol-z/inference-gateway`, is the **single source of truth** for what the product actually does. You will mine it carefully and write accurate docs against a fixed outline.

## Inputs and source-of-truth hierarchy

When two sources disagree, the higher one wins:

1. **The OpenAPI/Swagger file** (`docs/planning/openapi.yaml`), for endpoint paths, params, and request/response schemas. **If this file is not present yet, fall back to the code and mark every schema you infer with a TODO (see Gap Handling).**
2. **This repository's code**, for all behavior: features, endpoints, auth flows, privacy tiers, TEE attestation logic, billing, SDK and CLI surfaces, server tools, streaming format, errors, rate limits, config.
3. **`Nito_Docs_Section_Descriptions.md`**, for the documentation structure and the scope of each page. This defines what pages exist and what each one covers. Do not invent new top-level sections.
4. **Venice docs (https://docs.venice.ai/guides/overview) and OpenRouter docs (https://openrouter.ai/docs/quickstart)**, as **style and content-idea references**. Nito's protocol is close to both, so you may borrow from them at the level of *ideas*: how a page is shaped (intro, when-to-use, tabbed code samples, parameter tables, cross-links), how a concept is explained (for example how to teach prompt caching, tool calling, or structured outputs), and which sub-topics a good page on a given feature should cover. Use them to raise quality and completeness.

   Two hard limits on this borrowing:
   - **Rewrite everything in our own words and house voice. Never paste or lightly paraphrase their text.** Take the idea, then write it fresh. If a passage would read as close to their wording, rework it.
   - **Never import their product facts as Nito facts.** Their endpoints, parameters, model names, prices, rate limits, headers, and behaviors describe their products, not Nito. Every factual claim about Nito still comes from this repo or the OpenAPI file. If Venice or OpenRouter documents a capability, that is a prompt to check whether Nito's code has its own version, not license to state it exists.

   Our information architecture is our outline, not theirs.

## Non-negotiable rules

- **Code is truth. Never invent.** If a number, limit, price, model name, endpoint, or behavior is not in the code or the OpenAPI file, do not write it as fact. Mark it as a TODO.
- **Cite your source.** Every page you generate ends with an HTML comment listing the exact files and symbols the content was derived from, for example: `<!-- source: src/routes/chat.ts (chatCompletionsHandler), src/middleware/auth.ts -->`. This lets reviewers verify against the code.
- **House style.** No em dashes anywhere. Use periods, commas, colons, or parentheses. Keep sentences direct.
- **Privacy language stays "attested" or "verifiable," never "trustless."** Inference verifiability comes from TEE attestation. ZK secures on-chain action, not inference. Do not blur these.
- **Brand boundaries.** The product is **Nito**. The only reference to the underlying platform is a "Powered by Z" mark. On Nito doc surfaces, do not mention the Z token, validators, ZEC, staking, or crypto mechanics, except inside the single "Powered by Z" section (`8.11`). Selectable privacy is the model: do not claim all inference runs in TEEs. TEE is the highest tier only.
- **Do not document what does not exist.** If the outline lists a feature the code does not implement yet, create the page as a stub with a TODO and note "not yet implemented in code as of <commit hash>." Do not write fictional behavior.

---

## Scope and exclusions (this pass)

The native SDK, Agent SDK, CLI, and MCP server live in separate repositories that are **not** part of this run. Do not generate pages for them and do not fill them with TODO stubs. Specifically, skip these outline nodes for now:

- Get Started: the **native SDK**, **CLI**, and **MCP client** quickstarts. Keep the 60-seconds, OpenAI drop-in, Anthropic drop-in, and curl quickstarts.
- SDKs tab: **6.4 Native Nito SDK**, **6.5 Agent SDK**, **6.6 CLI (z)**, **6.7 MCP Server**. Keep 6.1 overview (but mark the SDK/CLI/MCP rows as "deferred"), 6.2 OpenAI drop-in, 6.3 Anthropic drop-in, 6.8 framework integrations, 6.9 editor integrations, and 6.10 Postman/Bruno, since those are documented against the gateway, not a separate SDK repo.
- Features: the **MCP (protocol overview)** page.
- Cookbook: any recipe that requires the native SDK, Agent SDK, CLI, or MCP server to follow (for example the Agent SDK driven builds). Keep recipes that can be done against the raw HTTP API.

For each skipped area, add a single line to a `docs/planning/DOCS_DEFERRED.md` file noting the node and the reason ("separate repo, out of scope for this pass"), so the team has a clean list of what to generate later. Do not let skipped surfaces block any in-scope page.

---

## Phase 1: Reconnaissance and fact extraction

Before writing any docs, build one extraction file: `docs/planning/DOCS_SOURCE_OF_TRUTH.md`.

Walk the repository and catalog the following, each entry citing the file path and symbol. Be exhaustive; this file is the spine for everything in Phase 2.

1. **Endpoints.** Every HTTP route: method, path (note both `/v1/...` and `/api/v1/...` variants), purpose, auth requirement, request params and body schema, response schema, status codes, and error shapes. Group by the API Reference 5.x structure in the outline.
2. **Models and providers.** How models are configured, the model ID convention (`provider/model[:tier]`), provider integrations (xAI, OpenRouter, NEAR, Phala), model groups, aliases, fallbacks, and backend selection logic. Note any privacy-tier-to-model mapping.
3. **Privacy tiers and TEE.** The T1 to T4 tier definitions as implemented, the per-call selection mechanism, the TEE attestation pipeline, the seven-gate baseline, freshness binding (`report_data = signing_address + nonce`), the address-only model-binding invariant, and per-provider attestation (NEAR domain, Phala appid). Capture the verify-yourself path.
4. **Data retention.** What is logged, retention windows (metadata-only default, idempotency replay, debug capture, saved chat), and any key-separation guarantees, as actually implemented.
5. **Auth and accounts.** API key creation and prefixes, management keys, OAuth (PKCE S256), social providers, sessions, CSRF, MFA, org and member model, permissions, and per-key rate limits.
6. **Billing and funding.** Credits, balance, usage, Stripe checkout and portal, x402 stored balance and inline paywall, subscriptions, auto top-up, promo codes, hold lifecycle, and failed-request billing behavior.
7. **Features.** Tool calling, server-side tools (web search, web fetch, datetime, file parser, code interpreter), structured outputs, reasoning, multimodal (which modalities are real), caching, message transforms, idempotency, BYOK, guardrails, app attribution, generation metadata, multi-model routing, MCP, webhooks.
8. **SDK, CLI, MCP surfaces (presence check only).** These live in separate repos and are out of scope this pass. Do not deeply document them. Only note, in one or two lines, whether this repo exposes anything relevant (for example the `/mcp` transport, OAuth scopes, or token prefixes the gateway itself defines), so the later SDK pass has a pointer. Do not generate SDK, Agent SDK, CLI, or MCP pages.
9. **Streaming, errors, rate limits.** The SSE and delta format, the canonical error envelope and code families, HTTP status mapping, rate-limit headers and values, retry and backoff behavior.
10. **Config and environment.** Base URLs, required env vars, headers, request IDs, pagination, versioning.

At the end of `DOCS_SOURCE_OF_TRUTH.md`, record the commit hash you analyzed and a list of anything the outline expects that you could not find in code.

**Pause here and print a short summary of what you found and what is missing, before starting Phase 2.**

---

## Phase 2: Page generation

Generate the documentation pages, one file per leaf item in `Nito_Docs_Section_Descriptions.md`, mirroring its structure.

### Output layout

Write pages under `docs/content/` mirroring the eight tabs, for example:
```
docs/content/get-started/introduction.mdx
docs/content/api-reference/chat-and-text/chat-completions.mdx
docs/content/privacy/tee-attestation/the-seven-gate-baseline.mdx
...
```
Use `.mdx`. Use kebab-case filenames. Create an index/overview page per tab and per numbered group. Keep all internal planning artifacts in `docs/planning/`, separate from `docs/content/`, so the published site never includes them.

### Writing quality and depth standard

These docs serve two audiences at once: developers integrating Nito, and a general or less-technical reader trying to understand what it does and why it matters. Every page must work for both. This is a primary goal, not a nice-to-have. Terse, reference-only pages are a failure here even when they are accurate.

Hold every page to these standards:

- **Explain, do not just list.** State what something is, why it exists, when to use it, and what happens if you get it wrong. A parameter table is the floor, not the page. Around it, write prose that teaches.
- **Lead for the general reader, deepen for the developer.** Open each page with a plain-language paragraph a non-engineer can follow (what this is and why it matters). Then go technical. A reader should be able to stop after the intro and still have learned something true.
- **Define terms on first use.** Spell out TEE, attestation, SSE, idempotency, embeddings, ZDR, and similar the first time they appear on a page, in one short clause. Assume the reader is smart but new to this specific topic.
- **Show, with realistic examples.** Every callable surface gets a complete, runnable-in-shape example with real-looking values, plus a short walk-through of what the request and response mean. Annotate non-obvious fields.
- **Anticipate the next question.** Add a short "Common mistakes" or "Tips" note where a developer is likely to trip (auth errors, wrong model ID, streaming parsing, rate-limit handling). Pair each with the fix.
- **Use concrete scenarios.** Where it helps, frame a feature around a real use case ("you are building a support bot that must not retain user data") rather than abstract description.
- **Depth target.** A typical feature or concept page runs roughly 400 to 900 words of prose plus examples. A pure endpoint reference page can be shorter, but still explains each field and shows a worked call. Never pad. Length should come from genuine explanation, not filler.
- **Readability.** Short paragraphs, descriptive headings, and a logical top-to-bottom flow. Bold sparingly. Tables for structured facts, prose for reasoning.

### Per-page template

Every page follows this anatomy (adapt sensibly for conceptual vs reference pages):

```mdx
---
title: <Page title>
description: <One-line summary, under 160 chars, no em dash>
---

<Intro: a plain-language opening a non-engineer can follow. What this is, why it matters, who it is for. 3 to 5 sentences. Expand the matching description in Nito_Docs_Section_Descriptions.md with real detail from the code.>

## How it works  (or a concept-appropriate heading)

<Prose that explains the mechanism or concept in depth. Define terms on first use. This is where the teaching happens, not just a table.>

## <Usage / Parameters / Steps as the page needs>

<For reference pages: a parameters table (name, type, required, description, and notes on non-obvious fields), built from the OpenAPI file or the code. Surround the table with prose that explains the important fields and the typical request shape.>

<Code samples in three tabbed runtimes when the page is a callable surface: curl, OpenAI SDK, Anthropic SDK. (The native Nito SDK runtime is deferred this pass; add it later when the SDK repo is in scope.) Derive them from real endpoint shapes, not guesses. Use realistic model IDs from the catalog. After the sample, briefly walk through what the request sends and what the response means.>

## Common mistakes  (or Tips)

<2 to 4 things a reader is likely to get wrong here, each with the fix. Skip only if genuinely nothing applies.>

## Notes

- Rate limits: <from code, or TODO>
- Retention: <from the privacy model, or TODO>
- Privacy-mode applicability: <which tiers support this, or TODO>

## Related

<Cross-links to the matching API Reference page, relevant Privacy tier, and any feature that pairs with this one.>

<!-- Try in Playground: TODO link once playground routes exist -->
<!-- source: <exact file paths and symbols> -->
```

### Generation rules

- Pull each page's scope from its description in `Nito_Docs_Section_Descriptions.md`. That description defines what belongs on the page. Expand it with verified detail from `DOCS_SOURCE_OF_TRUTH.md`.
- For the six canonical diagrams (request lifecycle, attestation verification, hold lifecycle, MCP OAuth flow, agent loop, privacy tier flow), insert a placeholder block describing what the diagram should show, marked as a TODO for design. Do not draw ASCII art in place of a real diagram.
- Cross-link related pages (for example, a Features page links to its API Reference page, and to the relevant Privacy tier).
- Keep code samples runnable in shape. Where a real value is unknown (a price, a specific limit), use a clearly labeled placeholder, never a fabricated number.
- **Before finishing each page, self-check against the writing quality and depth standard above.** Ask: would a non-engineer understand the intro, and would a developer have everything they need to ship? If the page is just a table and a code block, it is not done. Add the explanation, the worked example walk-through, and the common-mistakes note.

---

## Gap handling

Whenever a fact is not verifiable in the code or OpenAPI file, do both of these:

1. Inline, write a visible marker: `> **TODO (needs SME):** <what is missing and who likely owns it>`.
2. Append a row to `docs/planning/DOCS_GAPS.md` with: page path, the missing fact, the likely owner (Backend, Security, Finance, Legal, Design, Product), and why it could not be verified.

Treat these as always-TODO unless confirmed by code or OpenAPI: per-model pricing, rate-limit numbers, compliance claims (HIPAA-adjacent, SOC 2, EU residency), SLA terms, and any subprocessor list. Do not assert these from assumption.

---

## What not to do

- Do not invent pricing, limits, compliance status, or model names.
- Do not paste or lightly paraphrase text from Venice, OpenRouter, or any other docs site. Borrow ideas and structure, then write fresh in our own voice.
- Do not import another product's facts (endpoints, params, prices, limits, model names) as Nito's. Verify every Nito fact against this repo or the OpenAPI file.
- Do not expose Z token, validator, staking, or crypto mechanics on Nito surfaces, except in section 8.11.
- Do not claim all inference is TEE-sealed. Privacy is selectable across tiers.
- Do not use em dashes.
- Do not write behavior that the code does not implement.

---

## Deliverables

1. `docs/planning/DOCS_SOURCE_OF_TRUTH.md`, the extraction file, with the analyzed commit hash.
2. `docs/content/...`, the generated `.mdx` pages mirroring the outline.
3. `docs/planning/DOCS_GAPS.md`, every unverifiable fact with owner and reason.
4. `docs/planning/DOCS_DEFERRED.md`, the list of skipped SDK, Agent SDK, CLI, MCP, and dependent pages for a later pass.
5. A final summary printed to the console: pages generated, pages stubbed, pages deferred, total gaps by owner, and the top ten facts most worth resolving first.

Work in this order: Phase 1 fully, pause for the summary, then Phase 2 tab by tab in outline order (Get Started first). Commit after each tab with a clear message so the team can review incrementally.
