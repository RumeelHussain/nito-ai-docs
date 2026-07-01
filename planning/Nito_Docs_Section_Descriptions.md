# Nito Docs: Section Descriptions (node by node)

**Companion to:** `Docs_Outline_Handoff.md` and `Nito_Docs_Content_Generation_Guide.md`
**Purpose:** A separate description for every item in the outline, in the exact order and nesting of the handoff. Nothing is combined. Use it as the per-page brief: each line tells a writer what that one page covers.
**Style:** No em dashes. Privacy language stays attested or verifiable, never trustless. The only Z reference on Nito surfaces is the "Powered by Z" mark.

---

## 1. Get Started

- **Introduction (what Nito is + principles inline).** Defines Nito in one or two paragraphs and states the value: everything you can do with AI, now private. Folds the core principles into the page so there is no separate principles click.
- **How Nito Works (mental model + diagram).** Establishes the mental model with the request-lifecycle diagram: one key, many models, selectable privacy per call. The conceptual anchor the rest of the docs reference.
- **Quickstart, 60 seconds.** The fastest possible path to a first successful response, no SDK choice required. A single copy-paste block that proves the key works.
- **Quickstart, OpenAI SDK drop-in (Python + TypeScript, tabbed).** Shows that pointing the existing OpenAI SDK at Nito's base URL works unchanged. The lowest-friction path for OpenAI users.
- **Quickstart, Anthropic SDK drop-in (Python + TypeScript, tabbed).** The same drop-in path for the Anthropic SDK, with the base URL swap and any mapping notes. Meets Anthropic-based codebases where they are.
- **Quickstart, native SDK (Python + TypeScript, tabbed).** Installs the first-party Nito SDK and makes one call. The recommended path for new projects that want full feature coverage.
- **Quickstart, curl.** A raw HTTP example with headers and body spelled out. For readers who want no dependency or who work in another language.
- **Quickstart, CLI.** Installs the `z` CLI and runs one command to get a response. The terminal-first on-ramp.
- **Quickstart, MCP client (Claude Desktop, Cursor).** Connects Nito as an MCP server inside Claude Desktop and Cursor. The path for users who live in an MCP-aware client.
- **Authentication overview.** Explains how to obtain a key, the key types and prefixes, and the wallet option stated plainly. Orients the reader before they make a call and links to the full auth reference.
- **Make your first request.** Walks a new key through one non-streaming chat call end to end. The first concrete success after setup.
- **Stream your first response.** Repeats the first call with streaming enabled and shows how to read the token stream. Introduces streaming as a first-class default, not an advanced topic.
- **Check usage and credits.** Shows how to read the current balance and recent usage so a reader can confirm a call was billed. Closes the loop on the minimum working integration.
- **FAQ (inline at the bottom).** Answers common first-hour questions at the foot of the section, where they inherit the reader's context. Not a separate tab.

## 2. Models

- **Models overview.** Explains how model access works through one key and how to read a model card. The orientation page for the catalog.
- **Model catalog (live, filterable).** The interactive list of every available model with filters for modality, provider, and privacy tier. The page developers return to most often.
- **Model ID convention (provider/model[:tier]).** Documents the ID grammar and how the optional tier suffix selects privacy. Removes guesswork from every code sample.
- **Privacy tiers at a glance (T1 / T2 / T3 / T4).** A compact tier summary with links to the Privacy deep-dive. Enough to pick a tier without leaving the page.
- **Model families (single page with anchors).** Describes text, reasoning, embedding, vision, image gen, audio in and out, and video in and out, with what each family is for. Anchored so a reader can jump to one family.
- **Providers (xAI, OpenRouter, NEAR, Phala, single page with sections).** Profiles each serving provider and its privacy enforcement. Lets a reader reason about where a call actually runs.
- **Model groups (routing, aliases, fallbacks).** Explains grouped models, alias resolution, and fallback behavior. The basis for resilient model selection.
- **Backend selection.** How to influence or pin which backend serves a request. For callers with latency, cost, or privacy constraints.
- **Choosing the right model (decision guide).** A guided path that maps use cases to models and tiers. The "which one do I pick" page.
- **Pricing (master per-model table).** Per-model input and output cost, surfaced here and mirrored on each model card. The cost reference for model decisions.
- **Deprecations and lifecycle.** The policy for how models are introduced, deprecated, and retired, with timelines. Protects integrations from surprise removals.

## 3. Features

- **Chat completions.** The primary text-generation surface, its parameters, and a runnable example. The starting point for most builders.
- **Responses API.** The responses-style generation surface and how it differs from chat completions. For callers who want the newer response lifecycle.
- **Anthropic Messages.** The Anthropic-compatible messages surface on Nito. For teams standardized on the Anthropic message shape.
- **Streaming (SSE).** What streamed generation is at the capability level, with an example. Links to the wire-format details in API 5.16.
- **Tool calling (function calling).** Defining tools, the call-and-return loop, and parallel calls. The foundation for agentic behavior.
- **Server-side tools.** Tools Nito executes for you, invoked by parameter rather than a separate request. The intro that frames the five tools below.
  - **Web search.** Lets a model search the web mid-generation, with its privacy and cost note. For current-information tasks.
  - **Web fetch.** Retrieves and reads a specific URL during generation. For grounding answers in a known page.
  - **Datetime.** Gives a model reliable current date and time. Removes a common source of hallucination.
  - **File parser.** Extracts text and structure from uploaded files for use in a call. Backs document workflows.
  - **Code interpreter.** Runs code in a sandbox during generation. For computation, data work, and verification.
- **Structured outputs (JSON mode, JSON schema).** Forcing machine-parseable output, including schema enforcement and validation guidance. For callers who need reliable JSON.
- **Reasoning.** How to request and consume model reasoning, including effort controls. Pairs with the reasoning-tokens best practice.
- **Multimodal.** The capability home for everything beyond text. The intro that frames the seven modality pages below.
  - **Image input.** Sending images into a model and what formats are accepted. For vision and document tasks.
  - **PDF input.** Passing PDFs for analysis and extraction. For document-heavy workflows.
  - **Audio input.** Sending audio for transcription or understanding. For voice and media tasks.
  - **Video input.** Passing video for understanding. For richer media analysis.
  - **Image generation.** Producing images from prompts, with model and parameter notes. For creative and asset workflows.
  - **Audio generation.** Producing speech or audio output. For voice interfaces and media.
  - **Video generation.** Producing video output. For generative media workflows.
- **Embeddings.** Generating vectors for search and retrieval, with model and dimension notes. Inherits the same privacy guarantees as chat.
- **Prompt caching.** Reusing repeated prompt prefixes to cut cost and latency. For long-context and repeated-system-prompt workloads.
- **Response caching.** Returning a stored response for identical requests. For deduplicated or high-repeat traffic.
- **Message transforms.** Server-side transformations applied to messages before processing. For normalization and context shaping.
- **Idempotency.** Using idempotency keys so a retried request is not double-processed. For safe retries in production.
- **BYOK (Bring Your Own Key).** Bringing your own provider key and what it changes about billing and privacy. For enterprise control over upstream accounts.
- **Guardrails.** What Nito screens for and how to configure it. The intro that frames the two guardrail pages below.
  - **Prompt injection protection.** Detection of injection attempts in inputs and tool content. Defends agent and retrieval workflows.
  - **Sensitive information handling.** How sensitive data in requests is detected and treated. Supports privacy-safe usage.
- **App attribution and request metadata.** Attaching app identity and metadata to requests. For attribution, analytics, and routing.
- **Generation metadata.** Retrieving details about a completed generation after the fact. For auditing and debugging.
- **Multi-model routing.** Routing a request across multiple models by policy. For cost, quality, or availability optimization.
- **MCP (protocol overview, tool catalog, OAuth).** What MCP is on Nito, the tool catalog, and OAuth at the protocol level. The concept page; server install and config live under SDKs.
- **Webhooks (consuming Nito events).** What events Nito emits and why you would consume them. The capability view; the signature and retry spec is in API 5.18.

## 4. Privacy

- **Privacy overview.** The narrative entry point to the differentiator: what private means on Nito and why it matters. The page a security reviewer reads first.
- **Privacy architecture (diagram).** The end-to-end picture from request through inference to billing, as a diagram with prose. Shows where privacy is enforced at each hop.
- **Zero Data Retention.** What ZDR means on Nito, what is and is not retained, and how it is enforced. A headline claim stated precisely.
- **Per-call privacy modes (how selection works).** How a caller selects a tier per request and what changes at each level. Connects the abstract ladder to a concrete parameter.
- **Privacy tiers in depth.** The authoritative tier reference. The intro that frames the four tier pages below.
  - **T1: Anonymous.** The lightest tier: anonymized routing for low-sensitivity calls. Its exact guarantee, trade-offs, and supported models.
  - **T2: Private.** Privately served models with stronger guarantees than T1. What it protects and when to choose it.
  - **T3: Private+ (TEE).** TEE-sealed execution with attestation. The high-assurance tier and its verifiability story.
  - **T4: Encrypted (E2E TEE).** End-to-end encrypted execution inside a TEE, the top tier. The maximum-assurance guarantee and its constraints.
- **TEE attestation.** The verifiable-trust story that makes the privacy claim checkable. The intro that frames the seven pages below.
  - **What a TEE is (200 words).** A plain-language primer on trusted execution environments. The on-ramp for non-specialist readers.
  - **The attestation pipeline.** The end-to-end flow that produces and checks an attestation. The backbone of the TEE story.
  - **The seven-gate baseline.** The seven checks that must pass for an attestation to be valid. The pass-or-fail contract.
  - **Freshness binding (report_data = signing_address + nonce).** How freshness is bound into the report data to prevent replay. The anti-replay mechanism.
  - **Address-only model-binding invariant.** Why binding uses the address only and what that guarantees. A precise invariant security reviewers will scrutinize.
  - **Per-provider attestation (NEAR domain, Phala appid).** How attestation differs across providers, by NEAR domain and Phala appid. The provider-specific detail.
  - **Verify-yourself recipe (code sample).** Runnable code that verifies an attestation independently. Proof the reader can check the claim themselves.
- **What we see vs. providers see vs. never stored.** A three-column model of data visibility across parties. Removes ambiguity about who can access what.
- **Data retention model.** The precise retention contract. The intro that frames the five retention pages below.
  - **Metadata-only default.** The default state where only metadata is kept. The baseline retention posture.
  - **Idempotency replay (24h).** The 24-hour window where a request can be replayed idempotently. What is held and why.
  - **Governed debug capture (up to 7d).** Time-bounded, access-governed debug capture up to seven days. The exception path and its controls.
  - **Tenant saved chat (30d default, 90d cap).** Tenant-controlled chat retention with a 30-day default and 90-day cap. The user-facing retention dial.
  - **Three-distinct-keys guarantee.** The guarantee that three separate keys gate the retained data. The structural protection behind retention.
- **Provider-side privacy enforcement (xAI store:false, OpenRouter ZDR).** How upstream providers are constrained, with the concrete settings used. Shows privacy holds beyond Nito's own boundary.
- **Payment privacy.** How spend and payment stay private. The billing-side of the privacy story.
- **Threat model.** An explicit statement of what Nito does and does not protect. The intro that frames the two pages below.
  - **What Nito defends against.** The attacks and exposures Nito is designed to stop. The protection list.
  - **What Nito does not defend against.** The out-of-scope risks, stated plainly. Honesty that builds reviewer trust.
- **Compliance posture (HIPAA-adjacent, SOC 2, EU residency).** The current compliance status across these frameworks, separating what is true today from roadmap. Requires Legal and Security sign-off before publishing.
- **Data Processing Addendum (DPA).** The DPA and how to execute it. The procurement-facing legal artifact.
- **Known limitations.** A frank list of current gaps and constraints. Sets accurate expectations for evaluators.

## 5. API Reference

### 5.1 Overview

- **Base URL.** The canonical base URL and any regional variants. The first thing every integration needs.
- **Authentication.** How requests are authenticated at the wire level, including header format. The reference behind the Get Started auth page.
- **Headers.** Required and optional headers and what each controls. The complete header contract.
- **Request IDs.** How request IDs are issued and used for tracing and support. For debugging and correlation.
- **Pagination.** The pagination model used across list endpoints. So clients can iterate large results correctly.
- **Request format.** The canonical request envelope and conventions. The shared shape every endpoint assumes.
- **Response format.** The canonical response envelope and conventions. The shared shape every endpoint returns.
- **Error format.** The canonical error envelope at a glance, with a pointer to the full catalog in 5.17. So readers recognize errors immediately.
- **OpenAPI spec.** Where to find and how to use the machine-readable spec. The source of truth for generated clients.

### 5.2 Chat & Text

- **POST /v1/chat/completions.** The primary chat completion endpoint with full request and response schema. The most-called endpoint in the API.
- **POST /api/v1/chat/completions.** The api-prefixed variant of chat completions and how it differs. For clients on the api path.
- **POST /v1/responses (+ lifecycle sub-routes).** The responses endpoint and its lifecycle sub-routes. For the response-object workflow.
- **POST /v1/messages.** The Anthropic-style messages endpoint. For Anthropic-shaped integrations.
- **POST /v1/messages/count_tokens.** Counts tokens for a messages payload before sending. For budgeting and context management.
- **Finish reasons reference.** Every finish reason and what it signals. So clients handle completion states correctly.
- **Message format reference.** The full message object schema across roles and content types. The shared shape for all text endpoints.
- **Reasoning parameters.** The parameters that control reasoning behavior and output. The reference behind the Features reasoning page.

### 5.3 Embeddings

- **POST /v1/embeddings.** The embeddings endpoint with request and response schema. For generating vectors.
- **POST /api/v1/embeddings.** The api-prefixed embeddings variant. For clients on the api path.
- **Embedding models.** Which models are available for embeddings and their traits. For choosing the right embedder.
- **Vector dimensions.** The output dimensions per model and how to handle them. For index sizing and storage.
- **Input limits.** Token and size limits for embedding inputs. For chunking and batching decisions.

### 5.4 Multimodal Endpoints

- **Image input.** The wire-level contract for sending images, including encoding. The companion to the Features image-input page.
- **PDF input.** The wire-level contract for sending PDFs. For document workflows.
- **Audio input.** The wire-level contract for sending audio. For transcription and audio understanding.
- **Video input.** The wire-level contract for sending video. For video understanding.
- **Image generation.** The request and response schema for generating images. For asset workflows.
- **Audio generation.** The request and response schema for generating audio. For voice output.
- **Video generation.** The request and response schema for generating video. For generative media.

### 5.5 Models Catalog

- **GET /v1/models.** Lists available models on the v1 path. Powers programmatic discovery.
- **GET /api/v1/models.** The api-prefixed model list variant. For clients on the api path.
- **GET /api/v1/models/{id}/endpoints.** Lists the serving endpoints for a given model. For backend-aware routing.
- **GET /api/v1/model-groups.** Lists model groups and their members. Backs grouped routing and aliases.
- **GET /api/v1/providers.** Lists providers and their attributes. For provider-aware selection.

### 5.6 Generation Metadata

- **GET /api/v1/generation.** Retrieves metadata about a past generation. For auditing, debugging, and usage analysis.

### 5.7 Attestation

- **GET /api/v1/tee/attestation.** Returns a TEE attestation for verification. The machine-checkable side of the privacy story.
- **Seven-gate baseline reference.** The seven checks expressed as a reference for verifiers. The pass-or-fail contract in API form.
- **Verify-yourself recipe.** Runnable verification code at the reference level. So a developer can validate an attestation in their own stack.

### 5.8 Server / Built-in Tools

- **Web search.** The schema for invoking server-side web search. The reference behind the Features web-search page.
- **Web fetch.** The schema for invoking server-side web fetch. For URL grounding.
- **Datetime.** The schema for the datetime tool. For reliable time grounding.
- **File parser.** The schema for the file-parser tool. For document extraction.
- **Code interpreter.** The schema for the code-interpreter tool. For sandboxed computation.
- **Tool pricing.** How server-side tool usage is priced. For cost modeling.
- **Tool errors.** The error shapes specific to tools. For robust tool handling.

### 5.9 API Keys

- **Create API key.** Creates a new key with scopes. The provisioning entry point.
- **List API keys.** Lists existing keys. For management and audit.
- **Get API key.** Retrieves a single key's metadata. For inspection.
- **Update API key.** Updates a key's settings or scopes. For rotation and adjustment.
- **Delete API key.** Revokes a key. For offboarding and incident response.
- **Permissions.** The permission and scope model for keys. Defines what a key can do.
- **Per-key rate limits.** How rate limits are set and read per key. For tenant and agent isolation.
- **Management API keys.** The elevated keys that manage other keys and account resources. For programmatic administration.

### 5.10 Auth & Session

- **GET /api/v1/auth/status.** Returns current auth status. For client session checks.
- **GET /api/v1/auth/social/providers.** Lists supported social login providers. For building sign-in UIs.
- **GET /api/v1/csrf.** Returns a CSRF token. For protected browser flows.
- **GET /api/v1/session.** Returns the current session. For session-aware clients.
- **GET /api/v1/me/storage-key.** Returns the caller's storage key. For client-side encrypted storage.
- **Notifications.** The notifications surface tied to auth and session. For account-level messaging.
- **OAuth (PKCE S256) full reference.** The complete OAuth authorization-code with PKCE flow. The canonical sign-in reference for apps and agents.

### 5.11 Account Security

- **GET /api/v1/account/security.** Returns the account security state. The hub for hardening status.
- **Identity (email, wallet, disable).** Manages linked email and wallet identities, including disabling them. For account identity control.
- **MFA TOTP (setup, confirm, disable).** The TOTP multi-factor lifecycle. For stronger account protection.
- **OAuth tokens (list, revoke).** Lists and revokes issued OAuth tokens. For session and grant hygiene.
- **Sessions (list, revoke).** Lists and revokes active sessions. For device management and incident response.
- **Password.** Password set and change operations. For credential management.

### 5.12 Orgs & Members

- **GET /api/v1/orgs.** Lists organizations the caller belongs to. The entry point for multi-seat accounts.
- **GET /api/v1/orgs/{id}/api/overview.** Returns an org's API overview. For org-level usage at a glance.
- **GET /api/v1/orgs/{id}/free-tier.** Returns an org's free-tier status. For entitlement checks.
- **GET /api/v1/orgs/{id}/members.** Lists org members and roles. For team administration.

### 5.13 Usage & Credits

- **Get balance.** Returns the current credit balance. The first stop for spend checks.
- **Get usage.** Returns usage over a period. For reconciliation.
- **Usage analytics.** Aggregated usage analytics. For trend analysis and forecasting.
- **Billing events.** The stream of billing events. For accounting integration.
- **Credit history.** The history of credit grants and consumption. For audit.
- **Failed request billing behavior.** Documents whether and how failed requests are billed. Removes a common source of confusion.

### 5.14 Funding & Billing

- **POST /v1/funding/intents/{id}.** Creates or advances a funding intent. The start of an account top-up.
- **POST /v1/funding/stripe/checkout-sessions.** Opens a Stripe checkout session for funding. The hosted-payment path.
- **GET /api/v1/billing/checkout/products.** Lists purchasable products. For building a purchase UI.
- **POST /api/v1/billing/checkout/sessions.** Creates a billing checkout session. For initiating a purchase.
- **POST /api/v1/billing/portal/sessions.** Opens the customer billing portal. For self-serve billing management.
- **x402 stored balance.** Funding and spending against an x402 stored balance. The crypto-native top-up path.
- **x402 inline paywall.** Paying per request through an inline x402 paywall. For pay-as-you-go and agent payment.
- **Subscriptions.** Subscription creation and management. For recurring plans.
- **Auto top-up.** Automatic balance top-up rules. For uninterrupted service.
- **Promo codes.** Applying promotional codes. For campaigns and credits.

### 5.15 Files

- **POST /v1/files.** Uploads a file object. Backs multimodal and parser workflows.
- **GET /v1/files.** Lists uploaded files. For management.
- **GET /v1/files/{id}.** Retrieves a single file's metadata. For inspection.
- **DELETE /v1/files/{id}.** Deletes a file. For cleanup and retention control.

### 5.16 Streaming Format

- **Server-Sent Events.** The SSE transport Nito uses for streaming. The base of the streamed contract.
- **Delta format.** The shape of incremental deltas in a stream. So clients assemble output correctly.
- **Tool call streaming.** How tool calls appear within a stream. For streaming agent loops.
- **Error events.** How errors are delivered mid-stream. For robust stream handling.
- **Stream termination.** How a stream signals completion. So clients close cleanly.

### 5.17 Errors & Rate Limits

- **Canonical error envelope.** The single error object shape used everywhere. The reference all error notes point to.
- **Error codes by family.** Every error code grouped by family. For precise handling.
- **HTTP status mapping.** How error families map to HTTP status codes. For transport-level handling.
- **Rate limits.** The rate-limit model and headers. For staying within quota.
- **Retry behavior.** Which errors are retryable and how. For safe retries.
- **Backoff recommendations.** Recommended backoff strategy. For resilient clients.
- **Debugging requests.** How to use request IDs and tooling to debug. The page a developer hits when something breaks.

### 5.18 Webhooks

- **Webhook events.** The catalog of events Nito emits. So consumers know what to expect.
- **Signature verification.** How to verify webhook signatures. For trustworthy event handling.
- **Retry policy.** How failed deliveries are retried. For at-least-once handling.
- **Replay protection.** How to guard against replayed deliveries. For exactly-once semantics.

### 5.19 Request Builder / Playground

- **Interactive API console.** The in-docs console for live requests. The fastest path from reading to running.
- **Generate curl.** Exports the current request as curl. For copy-into-terminal.
- **Generate Python.** Exports the current request as Python. For copy-into-code.
- **Generate TypeScript.** Exports the current request as TypeScript. For copy-into-code.
- **Test models.** Swaps models in the console to compare. For quick evaluation.
- **Inspect responses.** Shows the full response, including headers and metadata. For understanding behavior.

## 6. SDKs

- **6.1 Overview & language matrix.** Which SDK to choose and what each supports across Python and TypeScript. The routing page for this tab.

### 6.2 OpenAI SDK drop-in (Python + TypeScript tabbed)

- **Base URL swap.** The single change that points the OpenAI SDK at Nito. The core of the drop-in.
- **Model mapping.** How OpenAI model names map to Nito model IDs. For correct routing.
- **Compatibility notes.** Behaviors that differ from native OpenAI usage. So readers avoid surprises.

### 6.3 Anthropic SDK drop-in (Python + TypeScript tabbed)

- **Base URL swap.** The change that points the Anthropic SDK at Nito. The core of the drop-in.
- **Mapping notes.** Model and parameter mapping plus any differences. For correct routing.

### 6.4 Native Nito SDK (Python + TypeScript, tabbed throughout)

- **Install.** Installing the SDK in Python and TypeScript. The first step.
- **Initialize client.** Creating and configuring the client. The setup every example assumes.
- **Chat completions.** Making chat calls through the SDK. The most common operation.
- **Responses.** Using the responses surface through the SDK. For the response workflow.
- **Streaming.** Consuming streams through the SDK. For real-time output.
- **Embeddings.** Generating embeddings through the SDK. For retrieval.
- **Multimodal.** Sending and receiving multimodal content through the SDK. For non-text workloads.
- **Models.** Listing and inspecting models through the SDK. For discovery in code.
- **Usage and credits.** Reading usage and balance through the SDK. For in-app metering.
- **Error handling.** Catching and handling SDK errors. For robust integrations.
- **Examples.** End-to-end SDK examples. The applied reference.

### 6.5 Agent SDK

- **Overview.** What the Agent SDK is and the problem it solves. The signature SDK surface.
- **When to use the Agent SDK.** Guidance on choosing the Agent SDK over the native SDK. For picking the right tool.
- **Agent architecture.** The architecture and primitives of an agent. The mental model for building.
- **Supported tools.** The tools an agent can use out of the box. For capability planning.
- **Privacy guarantees for agents.** The privacy properties that apply to agent runs. The on-thesis differentiator.
- **Installation (Python + TypeScript tabbed).** Installing the Agent SDK. The first step.
- **Environment setup.** Configuring environment and credentials. The pre-run checklist.
- **Building an agent.** The core build flow. The intro that frames the six pages below.
  - **Create a basic agent.** Standing up a minimal working agent. The hello-world.
  - **Add instructions.** Giving an agent its system instructions. For shaping behavior.
  - **Choose a model.** Selecting the model and tier for an agent. For cost and privacy fit.
  - **Run an agent loop.** Executing the agent's reason-and-act loop. The runtime core.
  - **Handle state.** Managing state across turns. For continuity.
  - **Handle memory.** Managing longer-term memory. For persistence across runs.
- **Tool execution.** How agents call tools. The intro that frames the six pages below.
  - **Define tools.** Declaring tools and their schemas. The starting point.
  - **Call tools.** Invoking tools within a run. The execution step.
  - **Validate tool inputs.** Validating arguments before execution. For safety.
  - **Return tool results.** Feeding results back to the model. For closing the loop.
  - **Stream tool calls.** Streaming tool activity. For live UIs.
  - **Handle tool errors.** Recovering from tool failures. For resilience.
- **Streaming.** Streaming an agent run. The intro that frames the four pages below.
  - **Stream agent responses.** Streaming the agent's final output. For responsive UX.
  - **Stream intermediate steps.** Surfacing reasoning and tool steps as they happen. For transparency.
  - **Stream tool calls.** Streaming tool invocations specifically. For tool-aware UIs.
  - **Handle cancellation.** Cancelling an in-flight run. For user control and cost.
- **Autonomous provisioning.** How an agent provisions and funds itself. The intro that frames the five pages below.
  - **Agent-created API keys.** An agent minting its own keys. For unattended operation.
  - **Agent-funded credits.** An agent funding its own balance. For self-sustaining workloads.
  - **Usage limits.** Capping what an agent can consume. For containment.
  - **Spend controls.** Bounding agent spend. For budget safety.
  - **Safety controls.** Guardrails on autonomous behavior. For safe autonomy.
- **Agent privacy.** The privacy view specific to agents. The intro that frames the five pages below.
  - **Prompt privacy.** How agent prompts are protected. For sensitive instructions.
  - **Tool privacy.** How tool inputs and outputs are protected. For sensitive tool data.
  - **Logging behavior.** What is and is not logged for agents. For auditability with privacy.
  - **Metadata visibility.** What metadata is visible and to whom. For exposure clarity.
  - **Agent billing records.** How agent spend is recorded privately. For private accounting.
- **Agent examples.** Worked agents end to end. The intro that frames the five pages below.
  - **Research agent.** A private research agent build. A flagship example.
  - **Coding agent.** A coding assistant build. For developer workflows.
  - **Support agent.** A customer support agent build. For service use cases.
  - **Personal assistant.** A personal assistant build. For consumer use cases.
  - **Data extraction agent.** A structured extraction agent build. For pipelines.

### 6.6 CLI (z)

- **Install (Homebrew, npm, direct binary).** The three install paths for the CLI. The first step.
- **Auth (z_cli_ OAuth).** Authenticating the CLI via OAuth with `z_cli_` tokens. The setup before use.
- **Commands.** The command set. The intro that frames the five pages below.
  - **z chat.** Running a chat from the terminal. The core interactive command.
  - **z models list.** Listing models from the terminal. For discovery.
  - **z keys.** Managing keys from the terminal. For provisioning.
  - **z usage.** Checking usage from the terminal. For spend.
  - **z attest.** Fetching and verifying an attestation from the terminal. For the privacy check.
- **Configuration (profiles, base URLs, env vars).** Configuring profiles, base URLs, and environment variables. For multi-environment work.
- **Scripting patterns (piping, JSON output, exit codes).** Patterns for using the CLI in scripts. For automation.

### 6.7 MCP Server

- **Connecting (Streamable HTTP at /mcp).** Connecting a client to the MCP server over Streamable HTTP. The entry point.
- **OAuth flow (PKCE S256, closed scope allowlist).** The MCP OAuth flow with a closed scope allowlist. For secure authorization.
- **Tokens (z_mcp_).** The `z_mcp_` token type and handling. For session management.
- **Tool catalog (z.* tools).** The `z.*` tools exposed over MCP. The capability surface.
- **Logging invariants.** What the MCP server does and does not log. For privacy-safe usage.
- **Quickstart clients.** Getting common clients connected. The intro that frames the three pages below.
  - **Claude Desktop.** Connecting Claude Desktop. A primary client.
  - **Cursor.** Connecting Cursor. A primary client.
  - **Generic MCP client.** Connecting any MCP-compliant client. The general path.

### 6.8 Framework integrations

- **LangChain / LangGraph.** Using Nito within LangChain and LangGraph. Setup plus example.
- **Vercel AI SDK.** Using Nito with the Vercel AI SDK. Setup plus example.
- **CrewAI.** Using Nito with CrewAI. Setup plus example.
- **Mastra.** Using Nito with Mastra. Setup plus example.
- **PydanticAI.** Using Nito with PydanticAI. Setup plus example.
- **LiteLLM.** Using Nito through LiteLLM. Setup plus example.
- **LlamaIndex.** Using Nito with LlamaIndex. Setup plus example.

### 6.9 Editor integrations

- **Cursor.** Configuring Nito in Cursor. For in-editor use.
- **Claude Code.** Configuring Nito with Claude Code. For in-editor use.
- **Codex.** Configuring Nito with Codex. For in-editor use.
- **OpenClaw.** Configuring Nito with OpenClaw. For gateway use.
- **Zed.** Configuring Nito in Zed. For in-editor use.
- **Continue.dev.** Configuring Nito in Continue.dev. For in-editor use.

- **6.10 Postman / Bruno collections.** Downloadable Postman and Bruno collections for exploring the API outside code. For testers and non-SDK workflows.

## 7. Cookbook

### 7.1 Getting Started Recipes

- **Quickstart recipe.** A runnable end-to-end first project. The cookbook on-ramp.
- **Migrate from OpenAI.** Moving an OpenAI app to Nito step by step. For OpenAI switchers.
- **Migrate from Anthropic.** Moving an Anthropic app to Nito. For Anthropic switchers.
- **Migrate from OpenRouter.** Moving an OpenRouter app to Nito. For the closest competitor's users.
- **Migrate from Together / Fireworks / Replicate / Perplexity.** Moving from other inference providers. For the broader switcher market.
- **Build your first chat app.** A complete minimal chat app. The foundational build.
- **Build a streaming chat UI.** A chat app with streamed responses. The real-time build.

### 7.2 Private AI Apps

- **Drop-in privacy upgrade.** Adding privacy to an existing app with minimal change. The easiest privacy win.
- **Build a private chat app.** A chat app built private from the start. The privacy-native baseline.
- **Build a private RAG bot.** A retrieval-augmented bot with privacy. A flagship build.
- **Build a private research assistant.** A research assistant with privacy. For knowledge work.
- **Build a document Q&A assistant.** A document question-answering app. For document workflows.
- **Build a structured data extraction pipeline.** A pipeline that extracts structured data privately. For data engineering.
- **TEE-attested call (T3) + verify nonce.** A T3 call that fetches and verifies the attestation nonce. Proof of the verifiable-privacy story.
- **End-to-end TEE call (T4).** A full T4 end-to-end encrypted call. The maximum-assurance build.
- **Confidential enterprise inference (BYOK + T3 + audited debug).** An enterprise pattern combining BYOK, T3, and audited debug capture. The deal-closing reference build.

### 7.3 Agents

- **Build a private research agent.** An end-to-end research agent. The applied Agent SDK flagship.
- **Build a coding assistant.** An end-to-end coding agent. For developer tooling.
- **Build a customer support agent.** An end-to-end support agent. For service use cases.
- **Build a tool-using agent.** An agent centered on tool use. For capability-rich agents.
- **Build an autonomous workflow agent.** An agent that runs a multi-step workflow. For automation.
- **Idempotent agent loop.** An agent loop made safe with idempotency. For reliable retries.
- **Agent with autonomous provisioning.** An agent that mints keys and funds itself. The self-sustaining build.

### 7.4 Bots & Integrations

- **Telegram bot.** A Nito-powered Telegram bot. A drop-in surface build.
- **Discord bot.** A Nito-powered Discord bot. A drop-in surface build.
- **WhatsApp assistant.** A Nito-powered WhatsApp assistant. A drop-in surface build.
- **Slack assistant.** A Nito-powered Slack assistant. A drop-in surface build.
- **OpenClaw-powered personal AI gateway.** A personal gateway built on OpenClaw and Nito. For power users.
- **Use Nito with Cursor.** A recipe for working in Cursor on Nito. For in-editor workflows.
- **Use Nito with Claude Code.** A recipe for working in Claude Code on Nito. For in-editor workflows.
- **Use Nito with Codex.** A recipe for working in Codex on Nito. For in-editor workflows.

### 7.5 Advanced Projects

- **Model comparison tool.** A tool that compares models side by side. A showcase build.
- **Usage dashboard.** A dashboard for usage. For operational visibility.
- **Cost-monitoring dashboard.** A dashboard focused on cost. For budget control.
- **Fallback router.** A router that fails over across models. For resilience.
- **Observability pipeline.** A pipeline exporting telemetry. For production monitoring.
- **Privacy-first AI workflow.** A workflow designed around the privacy tiers. The on-thesis showcase.
- **Multimodal app (image + audio + video).** An app combining several modalities. The full-capability showcase.

## 8. Resources

- **8.1 Pricing (master table).** The authoritative per-model pricing table. The page everything cost-related links to.
- **8.2 Rate limits.** The canonical rate-limit reference across surfaces. For quota planning.
- **8.3 Errors reference.** The full error catalog in one place. For lookups.

### 8.4 Best practices

- **Latency and performance.** Techniques to reduce latency and raise throughput. For fast apps.
- **Cost control.** Techniques to manage and reduce spend. For efficient apps.
- **Secure API key handling.** How to store and rotate keys safely. For security hygiene.
- **Privacy-safe logging.** How to log without leaking sensitive data. On-thesis operational guidance.
- **Uptime and fallbacks.** Designing for availability with fallbacks. For resilient apps.
- **Production readiness checklist.** A pre-launch checklist. The go-live gate.

### 8.5 Observability

- **Datadog.** Exporting telemetry to Datadog. For Datadog users.
- **Langfuse.** Exporting telemetry to Langfuse. For LLM-observability users.
- **Sentry.** Exporting errors to Sentry. For error tracking.
- **S3.** Exporting data to S3 or compatible storage. For data lakes.
- **Webhooks.** Using webhooks for observability. For custom pipelines.

- **8.6 Status page (per-surface uptime).** Live per-surface uptime. The health reference.

### 8.7 Trust Center

- **DPA.** The data processing addendum. The procurement artifact.
- **Subprocessors.** The current subprocessor list. For compliance review.
- **SLA.** The service level agreement. For enterprise commitments.
- **Security advisories.** Published advisories. For staying informed.
- **Responsible disclosure.** How to report vulnerabilities. For the security community.

### 8.8 Policies & Legal

- **Terms of Service.** The terms governing use. The legal baseline.
- **Acceptable Use Policy.** What use is and is not allowed. The conduct boundary.
- **Privacy Policy.** How data is handled. The privacy legal artifact.
- **Cookies.** The cookie policy. For web compliance.

- **8.9 FAQ (categorized).** A categorized FAQ across the product. The catch-all support surface.
- **8.10 Community.** Community entry points and channels. For peer support and engagement.

### 8.11 Powered by Z (discoverable footer path)

- **About the engine behind Nito.** Explains the Z engine underneath Nito, kept discoverable rather than loud. The single intentional Z exposure.
- **For developers building agents.** A path for developers who want the full Z surface for agents. The bridge from Nito to Z.

## Cross-Cutting Layers (apply to every reference page)

### Conventions

- **Four code-sample runtimes (curl, OpenAI SDK, Anthropic SDK, native Nito SDK).** Every reference page ships samples in all four. For copy-paste in any stack.
- **Request schema.** The request shape on every endpoint page. The contract in.
- **Response schema.** The response shape on every endpoint page. The contract out.
- **Error reference.** The relevant errors on every endpoint page. For handling.
- **Rate-limit notes.** The limits that apply to that endpoint. For quota awareness.
- **Retention notes.** What is retained for that endpoint. For privacy clarity.
- **Privacy-mode applicability (which tiers support this).** Which tiers the endpoint supports. For tier-aware building.
- **Playground link.** A one-click Try in Playground on every page. For instant testing.

### Diagrams (shared visual style)

- **Request lifecycle.** The path of a request through Nito. The orientation diagram.
- **Attestation verification.** How an attestation is verified. The TEE-trust diagram.
- **Hold lifecycle (billing).** How a billing hold is placed and released. The billing diagram.
- **MCP OAuth flow.** The MCP authorization flow. The MCP diagram.
- **Agent loop.** The agent reason-and-act cycle. The agent diagram.
- **Privacy tier flow.** How a call routes by privacy tier. The privacy diagram.

- **Search (tagged by surface: Chat / Embeddings / Privacy / Billing / MCP / CLI / Agent / Multimodal).** Site search tagged by surface so a reader filters to their area fast. The findability layer.
