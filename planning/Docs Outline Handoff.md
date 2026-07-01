# **Docs Outline**

## **Top Navigation**

Get Started | Models | Features | Privacy | API Reference | SDKs | Cookbook | Resources

Eight tabs. Casual to technical to operational, left to right.

---

## **Full Architecture Map**

docs.nito.ai  
│  
├── 1\. Get Started  
│   ├── Introduction (what Nito is \+ principles inline)  
│   ├── How Nito Works (mental model \+ diagram)  
│   ├── Quickstart, 60 seconds  
│   ├── Quickstart, OpenAI SDK drop-in (Python \+ TypeScript, tabbed)  
│   ├── Quickstart, Anthropic SDK drop-in (Python \+ TypeScript, tabbed)  
│   ├── Quickstart, native SDK (Python \+ TypeScript, tabbed)  
│   ├── Quickstart, curl  
│   ├── Quickstart, CLI  
│   ├── Quickstart, MCP client (Claude Desktop, Cursor)  
│   ├── Authentication overview  
│   ├── Make your first request  
│   ├── Stream your first response  
│   ├── Check usage and credits  
│   └── FAQ (inline at the bottom)  
│  
├── 2\. Models  
│   ├── Models overview  
│   ├── Model catalog (live, filterable)  
│   ├── Model ID convention (provider/model\[:tier\])  
│   ├── Privacy tiers at a glance (T1 / T2 / T3 / T4, links to Privacy deep-dive)  
│   ├── Model families (text, reasoning, embedding, vision, image gen, audio in/out, video in/out, single page with anchors)  
│   ├── Providers (xAI, OpenRouter, NEAR, Phala, single page with sections)  
│   ├── Model groups (routing, aliases, fallbacks)  
│   ├── Backend selection  
│   ├── Choosing the right model (decision guide)  
│   ├── Pricing (master per-model table)  
│   └── Deprecations and lifecycle  
│  
├── 3\. Features  
│   ├── Chat completions  
│   ├── Responses API  
│   ├── Anthropic Messages  
│   ├── Streaming (SSE)  
│   ├── Tool calling (function calling)  
│   ├── Server-side tools  
│   │   ├── Web search  
│   │   ├── Web fetch  
│   │   ├── Datetime  
│   │   ├── File parser  
│   │   └── Code interpreter  
│   ├── Structured outputs (JSON mode, JSON schema)  
│   ├── Reasoning  
│   ├── Multimodal  
│   │   ├── Image input  
│   │   ├── PDF input  
│   │   ├── Audio input  
│   │   ├── Video input  
│   │   ├── Image generation  
│   │   ├── Audio generation  
│   │   └── Video generation  
│   ├── Embeddings  
│   ├── Prompt caching  
│   ├── Response caching  
│   ├── Message transforms  
│   ├── Idempotency  
│   ├── BYOK (Bring Your Own Key)  
│   ├── Guardrails  
│   │   ├── Prompt injection protection  
│   │   └── Sensitive information handling  
│   ├── App attribution and request metadata  
│   ├── Generation metadata  
│   ├── Multi-model routing  
│   ├── MCP (protocol overview, tool catalog, OAuth)  
│   └── Webhooks (consuming Nito events)  
│  
├── 4\. Privacy  
│   ├── Privacy overview  
│   ├── Privacy architecture (diagram)  
│   ├── Zero Data Retention  
│   ├── Per-call privacy modes (how selection works)  
│   ├── Privacy tiers in depth  
│   │   ├── T1: Anonymous  
│   │   ├── T2: Private  
│   │   ├── T3: Private+ (TEE)  
│   │   └── T4: Encrypted (E2E TEE)  
│   ├── TEE attestation  
│   │   ├── What a TEE is (200 words)  
│   │   ├── The attestation pipeline  
│   │   ├── The seven-gate baseline  
│   │   ├── Freshness binding (report\_data \= signing\_address \+ nonce)  
│   │   ├── Address-only model-binding invariant  
│   │   ├── Per-provider attestation (NEAR domain, Phala appid)  
│   │   └── Verify-yourself recipe (code sample)  
│   ├── What we see vs. providers see vs. never stored  
│   ├── Data retention model  
│   │   ├── Metadata-only default  
│   │   ├── Idempotency replay (24h)  
│   │   ├── Governed debug capture (≤7d)  
│   │   ├── Tenant saved chat (30d default, 90d cap)  
│   │   └── Three-distinct-keys guarantee  
│   ├── Provider-side privacy enforcement (xAI store:false, OpenRouter ZDR)  
│   ├── Payment privacy  
│   ├── Threat model  
│   │   ├── What Nito defends against  
│   │   └── What Nito does not defend against  
│   ├── Compliance posture (HIPAA-adjacent, SOC 2, EU residency)  
│   ├── Data Processing Addendum (DPA)  
│   └── Known limitations  
│  
├── 5\. API Reference  
│   ├── 5.1 Overview  
│   │   ├── Base URL  
│   │   ├── Authentication  
│   │   ├── Headers  
│   │   ├── Request IDs  
│   │   ├── Pagination  
│   │   ├── Request format  
│   │   ├── Response format  
│   │   ├── Error format  
│   │   └── OpenAPI spec  
│   ├── 5.2 Chat & Text  
│   │   ├── POST /v1/chat/completions  
│   │   ├── POST /api/v1/chat/completions  
│   │   ├── POST /v1/responses (+ lifecycle sub-routes)  
│   │   ├── POST /v1/messages  
│   │   ├── POST /v1/messages/count\_tokens  
│   │   ├── Finish reasons reference  
│   │   ├── Message format reference  
│   │   └── Reasoning parameters  
│   ├── 5.3 Embeddings  
│   │   ├── POST /v1/embeddings  
│   │   ├── POST /api/v1/embeddings  
│   │   ├── Embedding models  
│   │   ├── Vector dimensions  
│   │   └── Input limits  
│   ├── 5.4 Multimodal Endpoints  
│   │   ├── Image input  
│   │   ├── PDF input  
│   │   ├── Audio input  
│   │   ├── Video input  
│   │   ├── Image generation  
│   │   ├── Audio generation  
│   │   └── Video generation  
│   ├── 5.5 Models Catalog  
│   │   ├── GET /v1/models  
│   │   ├── GET /api/v1/models  
│   │   ├── GET /api/v1/models/{id}/endpoints  
│   │   ├── GET /api/v1/model-groups  
│   │   └── GET /api/v1/providers  
│   ├── 5.6 Generation Metadata  
│   │   └── GET /api/v1/generation  
│   ├── 5.7 Attestation  
│   │   ├── GET /api/v1/tee/attestation  
│   │   ├── Seven-gate baseline reference  
│   │   └── Verify-yourself recipe  
│   ├── 5.8 Server / Built-in Tools  
│   │   ├── Web search  
│   │   ├── Web fetch  
│   │   ├── Datetime  
│   │   ├── File parser  
│   │   ├── Code interpreter  
│   │   ├── Tool pricing  
│   │   └── Tool errors  
│   ├── 5.9 API Keys  
│   │   ├── Create API key  
│   │   ├── List API keys  
│   │   ├── Get API key  
│   │   ├── Update API key  
│   │   ├── Delete API key  
│   │   ├── Permissions  
│   │   ├── Per-key rate limits  
│   │   └── Management API keys  
│   ├── 5.10 Auth & Session  
│   │   ├── GET /api/v1/auth/status  
│   │   ├── GET /api/v1/auth/social/providers  
│   │   ├── GET /api/v1/csrf  
│   │   ├── GET /api/v1/session  
│   │   ├── GET /api/v1/me/storage-key  
│   │   ├── Notifications  
│   │   └── OAuth (PKCE S256) full reference  
│   ├── 5.11 Account Security  
│   │   ├── GET /api/v1/account/security  
│   │   ├── Identity (email, wallet, disable)  
│   │   ├── MFA TOTP (setup, confirm, disable)  
│   │   ├── OAuth tokens (list, revoke)  
│   │   ├── Sessions (list, revoke)  
│   │   └── Password  
│   ├── 5.12 Orgs & Members  
│   │   ├── GET /api/v1/orgs  
│   │   ├── GET /api/v1/orgs/{id}/api/overview  
│   │   ├── GET /api/v1/orgs/{id}/free-tier  
│   │   └── GET /api/v1/orgs/{id}/members  
│   ├── 5.13 Usage & Credits  
│   │   ├── Get balance  
│   │   ├── Get usage  
│   │   ├── Usage analytics  
│   │   ├── Billing events  
│   │   ├── Credit history  
│   │   └── Failed request billing behavior  
│   ├── 5.14 Funding & Billing  
│   │   ├── POST /v1/funding/intents/{id}  
│   │   ├── POST /v1/funding/stripe/checkout-sessions  
│   │   ├── GET  /api/v1/billing/checkout/products  
│   │   ├── POST /api/v1/billing/checkout/sessions  
│   │   ├── POST /api/v1/billing/portal/sessions  
│   │   ├── x402 stored balance  
│   │   ├── x402 inline paywall  
│   │   ├── Subscriptions  
│   │   ├── Auto top-up  
│   │   └── Promo codes  
│   ├── 5.15 Files  
│   │   ├── POST /v1/files  
│   │   ├── GET /v1/files  
│   │   ├── GET /v1/files/{id}  
│   │   └── DELETE /v1/files/{id}  
│   ├── 5.16 Streaming Format  
│   │   ├── Server-Sent Events  
│   │   ├── Delta format  
│   │   ├── Tool call streaming  
│   │   ├── Error events  
│   │   └── Stream termination  
│   ├── 5.17 Errors & Rate Limits  
│   │   ├── Canonical error envelope  
│   │   ├── Error codes by family  
│   │   ├── HTTP status mapping  
│   │   ├── Rate limits  
│   │   ├── Retry behavior  
│   │   ├── Backoff recommendations  
│   │   └── Debugging requests  
│   ├── 5.18 Webhooks  
│   │   ├── Webhook events  
│   │   ├── Signature verification  
│   │   ├── Retry policy  
│   │   └── Replay protection  
│   └── 5.19 Request Builder / Playground  
│       ├── Interactive API console  
│       ├── Generate curl  
│       ├── Generate Python  
│       ├── Generate TypeScript  
│       ├── Test models  
│       └── Inspect responses  
│  
├── 6\. SDKs  
│   ├── 6.1 Overview & language matrix  
│   ├── 6.2 OpenAI SDK drop-in (recommended starting point, Python \+ TypeScript tabbed)  
│   │   ├── Base URL swap  
│   │   ├── Model mapping  
│   │   └── Compatibility notes  
│   ├── 6.3 Anthropic SDK drop-in (Python \+ TypeScript tabbed)  
│   │   ├── Base URL swap  
│   │   └── Mapping notes  
│   ├── 6.4 Native Nito SDK (Python \+ TypeScript, tabbed throughout)  
│   │   ├── Install  
│   │   ├── Initialize client  
│   │   ├── Chat completions  
│   │   ├── Responses  
│   │   ├── Streaming  
│   │   ├── Embeddings  
│   │   ├── Multimodal  
│   │   ├── Models  
│   │   ├── Usage and credits  
│   │   ├── Error handling  
│   │   └── Examples  
│   ├── 6.5 Agent SDK  
│   │   ├── Overview  
│   │   ├── When to use the Agent SDK  
│   │   ├── Agent architecture  
│   │   ├── Supported tools  
│   │   ├── Privacy guarantees for agents  
│   │   ├── Installation (Python \+ TypeScript tabbed)  
│   │   ├── Environment setup  
│   │   ├── Building an agent  
│   │   │   ├── Create a basic agent  
│   │   │   ├── Add instructions  
│   │   │   ├── Choose a model  
│   │   │   ├── Run an agent loop  
│   │   │   ├── Handle state  
│   │   │   └── Handle memory  
│   │   ├── Tool execution  
│   │   │   ├── Define tools  
│   │   │   ├── Call tools  
│   │   │   ├── Validate tool inputs  
│   │   │   ├── Return tool results  
│   │   │   ├── Stream tool calls  
│   │   │   └── Handle tool errors  
│   │   ├── Streaming  
│   │   │   ├── Stream agent responses  
│   │   │   ├── Stream intermediate steps  
│   │   │   ├── Stream tool calls  
│   │   │   └── Handle cancellation  
│   │   ├── Autonomous provisioning  
│   │   │   ├── Agent-created API keys  
│   │   │   ├── Agent-funded credits  
│   │   │   ├── Usage limits  
│   │   │   ├── Spend controls  
│   │   │   └── Safety controls  
│   │   ├── Agent privacy  
│   │   │   ├── Prompt privacy  
│   │   │   ├── Tool privacy  
│   │   │   ├── Logging behavior  
│   │   │   ├── Metadata visibility  
│   │   │   └── Agent billing records  
│   │   └── Agent examples  
│   │       ├── Research agent  
│   │       ├── Coding agent  
│   │       ├── Support agent  
│   │       ├── Personal assistant  
│   │       └── Data extraction agent  
│   ├── 6.6 CLI (z)  
│   │   ├── Install (Homebrew, npm, direct binary)  
│   │   ├── Auth (z\_cli\_ OAuth)  
│   │   ├── Commands  
│   │   │   ├── z chat  
│   │   │   ├── z models list  
│   │   │   ├── z keys  
│   │   │   ├── z usage  
│   │   │   └── z attest  
│   │   ├── Configuration (profiles, base URLs, env vars)  
│   │   └── Scripting patterns (piping, JSON output, exit codes)  
│   ├── 6.7 MCP Server  
│   │   ├── Connecting (Streamable HTTP at /mcp)  
│   │   ├── OAuth flow (PKCE S256, closed scope allowlist)  
│   │   ├── Tokens (z\_mcp\_)  
│   │   ├── Tool catalog (z.\* tools)  
│   │   ├── Logging invariants  
│   │   └── Quickstart clients  
│   │       ├── Claude Desktop  
│   │       ├── Cursor  
│   │       └── Generic MCP client  
│   ├── 6.8 Framework integrations  
│   │   ├── LangChain / LangGraph  
│   │   ├── Vercel AI SDK  
│   │   ├── CrewAI  
│   │   ├── Mastra  
│   │   ├── PydanticAI  
│   │   ├── LiteLLM  
│   │   └── LlamaIndex  
│   ├── 6.9 Editor integrations  
│   │   ├── Cursor  
│   │   ├── Claude Code  
│   │   ├── Codex  
│   │   ├── OpenClaw  
│   │   ├── Zed  
│   │   └── Continue.dev  
│   └── 6.10 Postman / Bruno collections  
│  
├── 7\. Cookbook  
│   ├── 7.1 Getting Started Recipes  
│   │   ├── Quickstart recipe  
│   │   ├── Migrate from OpenAI  
│   │   ├── Migrate from Anthropic  
│   │   ├── Migrate from OpenRouter  
│   │   ├── Migrate from Together / Fireworks / Replicate / Perplexity  
│   │   ├── Build your first chat app  
│   │   └── Build a streaming chat UI  
│   ├── 7.2 Private AI Apps  
│   │   ├── Drop-in privacy upgrade  
│   │   ├── Build a private chat app  
│   │   ├── Build a private RAG bot  
│   │   ├── Build a private research assistant  
│   │   ├── Build a document Q\&A assistant  
│   │   ├── Build a structured data extraction pipeline  
│   │   ├── TEE-attested call (T3) \+ verify nonce  
│   │   ├── End-to-end TEE call (T4)  
│   │   └── Confidential enterprise inference (BYOK \+ T3 \+ audited debug)  
│   ├── 7.3 Agents  
│   │   ├── Build a private research agent  
│   │   ├── Build a coding assistant  
│   │   ├── Build a customer support agent  
│   │   ├── Build a tool-using agent  
│   │   ├── Build an autonomous workflow agent  
│   │   ├── Idempotent agent loop  
│   │   └── Agent with autonomous provisioning  
│   ├── 7.4 Bots & Integrations  
│   │   ├── Telegram bot  
│   │   ├── Discord bot  
│   │   ├── WhatsApp assistant  
│   │   ├── Slack assistant  
│   │   ├── OpenClaw-powered personal AI gateway  
│   │   ├── Use Nito with Cursor  
│   │   ├── Use Nito with Claude Code  
│   │   └── Use Nito with Codex  
│   └── 7.5 Advanced Projects  
│       ├── Model comparison tool  
│       ├── Usage dashboard  
│       ├── Cost-monitoring dashboard  
│       ├── Fallback router  
│       ├── Observability pipeline  
│       ├── Privacy-first AI workflow  
│       └── Multimodal app (image \+ audio \+ video)  
│  
└── 8\. Resources  
    ├── 8.1 Pricing (master table)  
    ├── 8.2 Rate limits  
    ├── 8.3 Errors reference  
    ├── 8.4 Best practices  
    │   ├── Latency and performance  
    │   ├── Cost control  
    │   ├── Secure API key handling  
    │   ├── Privacy-safe logging  
    │   ├── Uptime and fallbacks  
    │   └── Production readiness checklist  
    ├── 8.5 Observability  
    │   ├── Datadog  
    │   ├── Langfuse  
    │   ├── Sentry  
    │   ├── S3  
    │   └── Webhooks  
    ├── 8.6 Status page (per-surface uptime)  
    ├── 8.7 Trust Center  
    │   ├── DPA  
    │   ├── Subprocessors  
    │   ├── SLA  
    │   ├── Security advisories  
    │   └── Responsible disclosure  
    ├── 8.8 Policies & Legal  
    │   ├── Terms of Service  
    │   ├── Acceptable Use Policy  
    │   ├── Privacy Policy  
    │   └── Cookies  
    ├── 8.9 FAQ (categorized)  
    ├── 8.10 Community  
    └── 8.11 Powered by Z (discoverable footer path)  
        ├── About the engine behind Nito  
        └── For developers building agents

### **Cross-Cutting Layers**

Every reference page  
├── Conventions  
│   ├── Four code-sample runtimes (curl, OpenAI SDK, Anthropic SDK, native Nito SDK)  
│   ├── Request schema  
│   ├── Response schema  
│   ├── Error reference  
│   ├── Rate-limit notes  
│   ├── Retention notes  
│   ├── Privacy-mode applicability (which tiers support this)  
│   └── Playground link  
├── Diagrams (shared visual style)  
│   ├── Request lifecycle  
│   ├── Attestation verification  
│   ├── Hold lifecycle (billing)  
│   ├── MCP OAuth flow  
│   ├── Agent loop  
│   └── Privacy tier flow  
└── Search (tagged by surface: Chat / Embeddings / Privacy / Billing / MCP / CLI / Agent / Multimodal)

---

## **Section Intent**

What each top-level tab is for, and what a reader should walk away with.

### **1\. Get Started**

The first ten minutes. A reader leaves with a working request from whichever entry point matches their stack (OpenAI SDK, Anthropic SDK, native SDK, curl, CLI, MCP). Mental model is established via “How Nito Works.” Principles are baked into the Introduction page, not a separate click. FAQ sits at the bottom of the section, not its own tab, because it inherits the reader’s context.

### **2\. Models**

A browseable, filterable surface. Treated as a first-class developer-curiosity destination, like OpenRouter’s catalog. The privacy tier ladder (T1 → T2 → T3 → T4) is taught here so it informs every model-selection decision. Pricing per model lives here (the master table is in Resources, but per-model cost surfaces on the model card too).

### **3\. Features**

What you can do, not how to call it. Each page explains the capability with a runnable example and links to the matching API Reference page. Multimodal lives here as a major subsection. MCP lives here as a protocol surface (the server install/config lives under SDKs).

### **4\. Privacy**

The differentiator. Promoted to top-level so a security reviewer can read it end to end without ever entering API Reference. Anchored by Zero Data Retention, the T1-T4 ladder, the verifiable TEE story (with a “verify it yourself” code recipe), the threat model, and the compliance posture. This section is what closes enterprise deals.

### **5\. API Reference**

Stripe-class reference density. Every endpoint gets request schema, response schema, error catalog, four-runtime code samples, rate-limit notes, retention notes, privacy-mode notes, and a one-click “Try in Playground” button. The Overview page establishes Base URL, Auth, Headers, Request IDs, Pagination, and links to the OpenAPI spec.

### **6\. SDKs**

Six ways in, ordered easiest-to-deepest: OpenAI drop-in, Anthropic drop-in, native Nito SDK, Agent SDK, CLI, MCP Server. Plus framework integrations, editor integrations, and Postman / Bruno collections. The Agent SDK is the signature surface and is documented at the same depth as the entire native SDK.

### **7\. Cookbook**

Project-based, code-first. Reader picks a goal, lands on a runnable end-to-end recipe with all dependencies installed and all gotchas noted. Migrations live here (not in their own tab) because they read as recipes.

### **8\. Resources**

Operational and reference. Pricing, rate limits, errors, observability, status, Trust Center, policies, FAQ, community, and the discoverable Powered by Z path. This is where production teams go.

---

## **Authoring Standards**

* **Code samples** ship in four runtimes per reference page: curl, OpenAI SDK, Anthropic SDK, native Nito SDK. Cookbook recipes ship in the language most natural for the task.

* **Every endpoint page** carries: request schema, response schema, error reference, rate-limit notes, retention notes, privacy-mode applicability, and Playground link.

* **Diagrams** reuse a single visual style across the docs. Six canonical diagrams: request lifecycle, attestation verification, hold lifecycle, MCP OAuth flow, agent loop, privacy tier flow.

* **Privacy claims must compile** against the canonical Privacy Model. Zero Data Retention is in scope. TEE language stays “attested” or “verifiable,” never “trustless.”

* **No em dashes.** Periods, commas, colons, or parentheses.

* **Live key autofill** in curl examples when the reader is logged into the console.

* **Versioning.** Single live version at launch.

* **Search.** Algolia DocSearch or equivalent. Tags by surface (Chat, Embeddings, Privacy, Billing, MCP, CLI, Agent, Multimodal).