# DOCS_DEFERRED.md

Outline nodes intentionally NOT generated in this pass. Two reasons appear:
- **separate repo** — lives in a repo outside `inference-gateway`; out of scope this pass.
- **not implemented** — listed in the outline but the gateway code returns 404 / has no route at commit `6ada3ccf`; revisit when built (or cut).

## Separate repo (SDK / Agent SDK / CLI / MCP server)
- Get Started: native SDK quickstart — separate repo, out of scope for this pass.
- Get Started: CLI quickstart — separate repo, out of scope for this pass.
- Get Started: MCP client quickstart (Claude Desktop, Cursor) — separate repo, out of scope for this pass.
- SDKs 6.4 Native Nito SDK — separate repo, out of scope for this pass.
- SDKs 6.5 Agent SDK (all sub-pages) — separate repo, out of scope for this pass.
- SDKs 6.6 CLI (`z`) (all sub-pages) — separate repo, out of scope for this pass.
- SDKs 6.7 MCP Server (all sub-pages) — separate repo, out of scope for this pass.
- Features: MCP (protocol overview) page — separate repo (server lives elsewhere); gateway only exposes the `/mcp` transport.
- Cookbook 7.3 Agents (all) — require Agent SDK; separate repo.
- Cookbook recipes that require native SDK / CLI / MCP server to follow — separate repo.

Pointers the gateway DOES define for the later SDK pass: `/mcp` Streamable HTTP transport (default path `/mcp`); OAuth scopes `cli:inference`, `mcp:inference`, `profile:org`; token prefixes `z_sk_`, `nito_cli_`, `z_cli_`, `z_mcp_`; OAuth clients `nito-cli`, `z-mcp`.

In scope and KEPT (documented against the gateway, not a separate repo): Get Started 60-seconds / OpenAI drop-in / Anthropic drop-in / curl; SDKs 6.1 overview (mark SDK/CLI/MCP rows "deferred"), 6.2 OpenAI drop-in, 6.3 Anthropic drop-in, 6.8 framework integrations, 6.9 editor integrations, 6.10 Postman/Bruno; Cookbook recipes doable against raw HTTP.

## Not implemented at this commit (stub-or-cut — Product decision pending)
- Features: Responses API; Anthropic Messages — `/v1/responses`, `/v1/messages` return 404.
- API 5.2: `POST /v1/responses` (+ lifecycle), `POST /v1/messages`, `POST /v1/messages/count_tokens` — 404.
- API 5.4 / Features Multimodal: Audio input, Video input, Audio generation, Video generation — no routes.
- API 5.5: `GET /api/v1/models/{id}/endpoints`, `GET /api/v1/providers` — no routes.
- API 5.6 / Features Generation metadata: `GET /api/v1/generation` — no route.
- API 5.14: `POST /v1/funding/intents/{id}`, `POST /v1/funding/stripe/checkout-sessions` — no routes (real funding under `/api/v1/billing/*`).
- API 5.15: Files (`/v1/files`, `/v1/files/*`) — 404.
- API 5.18 / Features Webhooks: outbound consumer webhooks — no event-delivery system.
- API 5.19 / Playground: no playground routes exist yet.
- Features: Datetime tool, Structured outputs (schema enforcement), Response caching, Message transforms, BYOK, Guardrails (prompt injection / sensitive info) — absent or not exposed on the HTTP surface.
