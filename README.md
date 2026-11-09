# Best Agent Gateways

An agent gateway is a control point between the callers that delegate work and the remote agents that perform it. It authenticates each caller, decides which agents that caller can reach, forwards the task, and records the exchange. Most products in this category implement the [Agent2Agent (A2A) protocol](https://a2a-protocol.org/latest/specification/#1-introduction), and they differ in how deeply they understand it.

This list selects 11 agent gateways that meet the criteria below. Each entry gives the license and deployment model, what the gateway does well, a real limitation, and who it suits. The full catalog is [Awesome AI Gateways](https://github.com/ai-gateways/awesome-ai-gateways#agent-gateways), and the [agent gateway guide](https://github.com/ai-gateways/agent-gateway#what-is-an-agent-gateway) explains how the technology works.

## Contents

- [How these agent gateways were selected](#how-these-agent-gateways-were-selected)
- [Agent gateway comparison](#agent-gateway-comparison)
- [Best open-source agent gateways](#best-open-source-agent-gateways)
  - [Bifrost](#bifrost)
  - [LiteLLM](#litellm)
  - [agentgateway](#agentgateway)
  - [ContextForge](#contextforge)
  - [Jarvis Registry](#jarvis-registry)
  - [MCP Gateway and Registry](#mcp-gateway-and-registry)
- [Best commercial agent gateways](#best-commercial-agent-gateways)
  - [Kong AI Gateway](#kong-ai-gateway)
  - [Azure API Management](#azure-api-management)
  - [Amazon Bedrock AgentCore Gateway](#amazon-bedrock-agentcore-gateway)
  - [Gravitee](#gravitee)
  - [MuleSoft Agent Fabric](#mulesoft-agent-fabric)
- [How to choose an agent gateway](#how-to-choose-an-agent-gateway)
- [What this list leaves out](#what-this-list-leaves-out)
- [Related guides](#related-guides)

## How these agent gateways were selected

A platform is on this list only when its own repository or documentation shows that it meets every test below. Features in a paid edition count toward the tests, and each entry says which ones are paid.

- **Routes agent-to-agent calls.** Callers reach A2A agents through the gateway, including agents that run outside the vendor's own runtime.
- **Caller authentication.** The gateway authenticates callers with keys, tokens, OAuth, or single sign-on.
- **Per-agent access.** Access can be limited so that a caller reaches only specific agents, by key, team, group, role, or policy.
- **Agent-call visibility.** Logs, metrics, or traces record calls to agents.
- **Adoption and upkeep.** Open-source projects need at least 1,000 GitHub stars and a commit in the last six months. Commercial products need public documentation, a self-serve signup or free trial, and an established cloud or API platform.

## Agent gateway comparison

The table compares the 11 gateways on license, deployment, A2A handling, caller authentication, per-agent access, and other traffic they govern. Each entry below links the source for its row.

| Gateway | License | Deployment | A2A handling | Caller authentication | Per-agent access | Also governs |
|---|---|---|---|---|---|---|
| [Bifrost](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/overview.mdx#L19) | Apache 2.0 | Self-hosted | JSON-RPC, REST, and gRPC, with client and agent free to differ | Virtual keys; enterprise identities are Enterprise | Per virtual key; access profiles and projects are Enterprise | LLM, MCP |
| [LiteLLM](https://docs.litellm.ai/docs/a2a#:~:text=Manage%20which%20Teams) | MIT, except `enterprise/` | Self-hosted | Send and stream through the proxy; other methods forwarded | Virtual keys | Per key or team | LLM, MCP |
| [agentgateway](https://agentgateway.dev/docs/standalone/latest/documentation/agent/a2a/) | Apache 2.0 | Self-hosted binary or Kubernetes | A2A proxy | JWT, API keys, OAuth | CEL rules on routes | LLM, MCP |
| [ContextForge](https://ibm.github.io/mcp-context-forge/using/agents/a2a/#agent-tool-state-cascade) | Apache 2.0 | PyPI, Docker, or Kubernetes | Agents exposed as MCP tools | JWT, Basic, SSO | Virtual servers | MCP, REST, gRPC |
| [Jarvis Registry](https://jarvisregistry.com/features/a2a-gateway-registry/#how-ai-clients-use-registered-agents) | Apache 2.0 | Self-hosted | Registry with a shared gateway endpoint | OAuth or OIDC | Per-agent ACLs, private by default | MCP |
| [MCP Gateway and Registry](https://github.com/agentic-community/mcp-gateway-registry/blob/main/docs/gateway-proxy-operational-guide.md#pattern-4--a2a-agents) | Apache 2.0 | Docker Compose, EKS, or ECS | Optional proxy route; peer-to-peer by default | OAuth against an identity provider | Group access rules | MCP, LLM |
| [Kong AI Gateway](https://developer.konghq.com/ai-gateway/a2a/#route-a2a-traffic-through-ai-gateway) | Apache 2.0 core; A2A plugin Enterprise | Self-hosted or Konnect | JSON-RPC and REST, with Agent Card rewriting | OpenID Connect through AI policies | AI policies | LLM, MCP |
| [Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/agent-to-agent-api#key-capabilities) | Proprietary | Azure service | JSON-RPC only, with Agent Card rewriting | Subscription keys and policies | Per API | LLM, MCP |
| [Amazon Bedrock AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-http-passthrough.html#gateway-target-http-passthrough) | Proprietary | AWS service | HTTP passthrough, no protocol translation | Inbound authentication | Gateway policies | LLM, MCP |
| [Gravitee](https://documentation.gravitee.io/apim/ai-agent-management#a2a-proxy) | Proprietary (Enterprise Edition) | Self-hosted, hybrid, or SaaS | A2A proxy and agent catalog | Gateway security policies | Plans and policies | LLM, MCP |
| [MuleSoft Agent Fabric](https://docs.mulesoft.com/gateway/latest/agent-policies#a2a-v1-policies) | Proprietary | Omni Gateway on Anypoint Platform | A2A policies, including v1.0 | Gateway authentication policies | Gateway authorization policies | LLM, MCP |

Details reflect each project's repository or documentation in November 2026; confirm current capabilities before choosing.

## Best open-source agent gateways

Six open-source projects meet the criteria. Their A2A support ranges from routing the protocol natively to presenting agents as MCP tools or adding a proxy route to a registry, so check how callers will reach your agents.

### Bifrost

[Bifrost](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/overview.mdx#L19), built by Maxim AI, registers A2A agents and governs calls to them under the same virtual keys as [model calls](https://github.com/maximhq/bifrost#key-features) and [MCP tool calls](https://github.com/maximhq/bifrost/blob/dev/docs/mcp/overview.mdx#L134).

- **License and deployment:** [Apache 2.0](https://github.com/maximhq/bifrost#license). Self-hosted through [npx, Docker, or the Go SDK](https://github.com/maximhq/bifrost#getting-started-options), or on [Kubernetes and other container platforms](https://github.com/maximhq/bifrost/blob/dev/docs/deployment-guides/overview.mdx#L11).
- **Does well:** [Registers agents from their Agent Cards](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/registration.mdx#L15), [grants agent access per virtual key](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/access-control.mdx#L17) separately from model and tool permissions, and lets [client and agent use different transports](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/transports-and-recovery.mdx#L9). [Agent Logs group operations by session](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/observability.mdx#L31).
- **Limitation:** [Access profiles, projects, user attribution, and scoped log access](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/overview.mdx#L61) are Enterprise features. [Task state is not copied between interfaces](https://github.com/maximhq/bifrost/blob/dev/docs/features/agent-gateway/transports-and-recovery.mdx#L39), so a transport switch works only if the new endpoint can reach the same task.
- **Suits:** Platform teams that want agent, model, and tool traffic in one self-hosted gateway with separate grants for each.

### LiteLLM

[LiteLLM](https://docs.litellm.ai/docs/a2a#:~:text=Manage%20which%20Teams) adds an agent gateway to its proxy server, so A2A agents are invoked and logged alongside model calls.

- **License and deployment:** [MIT, except the `enterprise/` directory](https://github.com/BerriAI/litellm/blob/main/LICENSE#L3). Self-hosted with [Docker or Terraform](https://github.com/BerriAI/litellm#get-started).
- **Does well:** [Agent access per key or team](https://docs.litellm.ai/docs/a2a_agent_permissions#:~:text=Control%20which%20A2A%20agents), [logging and load balancing](https://docs.litellm.ai/docs/a2a#:~:text=Manage%20which%20Teams), and support for A2A agents as well as Vertex AI Agent Engine, LangGraph, Azure AI Foundry, Bedrock AgentCore, and Pydantic AI.
- **Limitation:** [Logging, guardrails, and spend tracking apply to send and stream methods](https://docs.litellm.ai/docs/a2a#:~:text=All%20other%20methods%20are%20forwarded), while other A2A methods are forwarded upstream. [Single sign-on and audit logs](https://docs.litellm.ai/docs/enterprise#:~:text=single%20sign-on%2C%20audit%20logs) require the Enterprise license.
- **Suits:** Teams already running the LiteLLM proxy that want agents under the same keys and teams.

### agentgateway

[agentgateway](https://github.com/agentgateway/agentgateway#key-features) is a Rust proxy and [Linux Foundation](https://github.com/agentgateway/agentgateway#:~:text=Linux%20Foundation) project built on MCP and A2A.

- **License and deployment:** Apache 2.0. Self-hosted as a [standalone binary or on Kubernetes](https://github.com/agentgateway/agentgateway#documentation).
- **Does well:** [Proxies requests to A2A agents](https://agentgateway.dev/docs/standalone/latest/documentation/agent/a2a/) in the same data plane as LLM and MCP traffic, with JWT, API key, and OAuth authentication and OpenTelemetry output.
- **Limitation:** The docs offer dedicated authorization for MCP, while access to an agent uses [general HTTP authorization](https://agentgateway.dev/docs/standalone/latest/documentation/configuration/security/http-authz/) on its route, so per-agent rules are written as policy on that route.
- **Suits:** Kubernetes platform teams that want one open-source data plane for model, tool, and agent traffic.

### ContextForge

[ContextForge](https://github.com/IBM/mcp-context-forge#overview--goals), from IBM, is a registry and proxy that federates MCP servers, A2A agents, and REST or gRPC APIs into one endpoint.

- **License and deployment:** Apache 2.0. Installs from PyPI or Docker and scales to Kubernetes with Redis-backed federation.
- **Does well:** [Virtual servers](https://ibm.github.io/mcp-context-forge/using/agents/a2a/#virtual-server-integration) control access to agents through server-specific endpoints, with [per-agent metrics](https://ibm.github.io/mcp-context-forge/using/agents/a2a/#monitoring-and-metrics) and [encrypted credentials and audit logging](https://ibm.github.io/mcp-context-forge/using/agents/a2a/#security-considerations).
- **Limitation:** [Each registered agent becomes an MCP tool](https://ibm.github.io/mcp-context-forge/using/agents/a2a/#agent-tool-state-cascade), so callers reach agents through MCP rather than native A2A, and the [default configuration allows up to 100 agents](https://ibm.github.io/mcp-context-forge/using/agents/a2a/#configuration).
- **Suits:** Teams whose callers are MCP clients and that want to reach A2A agents from them.

### Jarvis Registry

[Jarvis Registry](https://github.com/ascending-llc/jarvis-registry#what-is-jarvis-registry), from ASCENDING and [built on MCP Gateway and Registry](https://github.com/ascending-llc/jarvis-registry#acknowledgments), is a self-hosted gateway and registry for MCP servers, A2A agents, and skills.

- **License and deployment:** [Apache 2.0](https://github.com/ascending-llc/jarvis-registry#license). Self-hosted, with [deployment guides](https://github.com/ascending-llc/jarvis-registry#deployment) for EKS, AKS, GKE, and Docker Compose.
- **Does well:** [Clients discover the agents they are allowed to see and invoke them through a shared endpoint](https://jarvisregistry.com/features/a2a-gateway-registry/#how-ai-clients-use-registered-agents). [Agents are private until access is granted](https://jarvisregistry.com/features/a2a-gateway-registry/#resource-sharing-access-control), and the platform adds OAuth or OIDC sign-in, request logging, and OpenTelemetry tracing.
- **Limitation:** The [documented ACL levels](https://jarvisregistry.com/features/a2a-gateway-registry/#permission-levels) are view, edit, and owner, with no separate level for invoking an agent.
- **Suits:** Teams that want a catalog of agents and MCP servers with discovery built in.

### MCP Gateway and Registry

[MCP Gateway and Registry](https://github.com/agentic-community/mcp-gateway-registry#whats-in-the-box) is a governed entry point and registry for MCP servers, agents, and skills.

- **License and deployment:** [Apache 2.0](https://github.com/agentic-community/mcp-gateway-registry#license). Deploys with Docker Compose, on Amazon EKS with Helm, or on Amazon ECS with Terraform.
- **Does well:** Agent discovery by capability, OAuth against an existing identity provider, [group-based access rules on gateway routes](https://github.com/agentic-community/mcp-gateway-registry/blob/main/docs/gateway-proxy-operational-guide.md#authorizing-callers), and audit logs.
- **Limitation:** [A2A traffic runs peer-to-peer by default](https://github.com/agentic-community/mcp-gateway-registry#how-it-works), [proxying ships off](https://github.com/agentic-community/mcp-gateway-registry/blob/main/docs/gateway-proxy-operational-guide.md#1-enable-the-feature), and a [proxied agent gets a route without streaming or custom header settings](https://github.com/agentic-community/mcp-gateway-registry/blob/main/docs/gateway-proxy-operational-guide.md#pattern-4--a2a-agents).
- **Suits:** Teams that mainly need agent discovery and access decisions, with direct agent-to-agent calls.

## Best commercial agent gateways

These products add A2A to an established cloud or API management platform. They suit teams already running that platform, and A2A is tied to its licensing and configuration model.

### Kong AI Gateway

[Kong AI Gateway](https://developer.konghq.com/ai-gateway/a2a/#route-a2a-traffic-through-ai-gateway) routes A2A traffic alongside API, LLM, and MCP traffic.

- **License and deployment:** [Apache 2.0](https://github.com/Kong/kong#license) for Kong Gateway; the A2A plugin is part of AI Gateway Enterprise. [Self-hosted](https://github.com/Kong/kong#getting-started) or on Kong Konnect.
- **Does well:** Detects JSON-RPC and REST bindings, [rewrites Agent Card URLs to the gateway address](https://developer.konghq.com/plugins/ai-a2a-proxy/#agent-card-url-rewriting), [secures endpoints through AI policies](https://developer.konghq.com/ai-gateway/a2a/#secure-a2a-endpoints), and sends [A2A metrics and OpenTelemetry attributes](https://developer.konghq.com/plugins/ai-a2a-proxy/#logging-and-observability) to analytics.
- **Limitation:** The plugin is [only available in AI Gateway Enterprise](https://developer.konghq.com/plugins/ai-a2a-proxy/#:~:text=only%20available%20as%20part%20of%20our%20AI%20Gateway%20Enterprise), and [A2A detection is skipped for JSON-RPC bodies above a configurable limit](https://developer.konghq.com/plugins/ai-a2a-proxy/#request-body-size), 1 MB by default.
- **Suits:** Enterprise Kong customers that want agent traffic under existing gateway policy.

### Azure API Management

[Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/agent-to-agent-api#key-capabilities) imports A2A agents as APIs and governs them alongside model, MCP, and REST APIs.

- **License and deployment:** Proprietary. An Azure service.
- **Does well:** Mediates JSON-RPC operations to the A2A backend, rewrites the Agent Card to the gateway host, [applies policies per agent API](https://learn.microsoft.com/en-us/azure/api-management/agent-to-agent-api#configure-policies-for-the-a2a-agent-api), [supports subscription keys](https://learn.microsoft.com/en-us/azure/api-management/agent-to-agent-api#configure-subscription-key-authentication), and adds OpenTelemetry agent attributes through Application Insights.
- **Limitation:** [Only JSON-RPC-based A2A agent APIs are supported](https://learn.microsoft.com/en-us/azure/api-management/agent-to-agent-api#limitations), and outgoing response bodies cannot be deserialized.
- **Suits:** Organizations that already standardize on Azure API Management.

### Amazon Bedrock AgentCore Gateway

[Amazon Bedrock AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html#gateway-core-concepts-connectivity-layer) is a managed AWS entry point for agent, tool, and model traffic, and it fronts A2A agents through HTTP passthrough targets.

- **License and deployment:** Proprietary. A managed AWS service.
- **Does well:** [Passthrough targets](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-http-passthrough.html#gateway-target-http-passthrough) put agent URLs behind centralized authentication, policy enforcement, and observability, with the [gateway managing inbound authentication and outbound credentials](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-http-passthrough.html#gateway-target-http-passthrough-auth).
- **Limitation:** AWS only. The [gateway does not perform protocol translation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-http-passthrough.html#gateway-target-http-passthrough-config), so clients must use the protocol the target implements.
- **Suits:** AWS teams that want partner or internal agents behind a managed endpoint.

### Gravitee

[Gravitee](https://documentation.gravitee.io/apim/ai-agent-management#a2a-proxy) is an API management platform whose Agent Gateway includes an A2A proxy and an LLM proxy, alongside an agent catalog.

- **License and deployment:** Proprietary for these features. Self-hosted, hybrid, or SaaS, with a [14-day free trial](https://www.gravitee.io/try-gravitee#:~:text=Try%20all%20enterprise%20APIM%20features).
- **Does well:** Applies [security, policies, plans, and observability](https://documentation.gravitee.io/apim/ai-agent-management#:~:text=add%20security%2C%20policies%2C%20plans%2C%20and%20observability) to calls between agents, and adds an [agent catalog](https://documentation.gravitee.io/apim/ai-agent-management#agent-catalog) for discovery.
- **Limitation:** [AI Agent Management is part of the Enterprise Edition](https://documentation.gravitee.io/apim/readme/enterprise-edition#ai-agent-management), so the open-source API Management distribution does not include these features.
- **Suits:** Teams already on Gravitee that want agents, APIs, and events under one platform.

### MuleSoft Agent Fabric

[MuleSoft Agent Fabric](https://www.mulesoft.com/platform/agent-governance#:~:text=Agent%20Fabric%20applies%20one%20set%20of%20policies) applies one set of policies to agents, MCP servers, and models through Omni Gateway on the Anypoint Platform.

- **License and deployment:** Proprietary. Runs on the Anypoint Platform, with a [30-day free trial](https://www.mulesoft.com/platform/agent-governance#:~:text=free%20for%2030%20days).
- **Does well:** Omni Gateway offers [A2A policies](https://docs.mulesoft.com/gateway/latest/agent-policies#a2a-policies) for Agent Card rewriting and schema validation, plus [authentication, authorization, monitoring, and logging policies](https://docs.mulesoft.com/gateway/latest/agent-policies#:~:text=rate%20limiting%2C%20authentication%2C%20authorization) on A2A server instances.
- **Limitation:** The docs list [separate policy sets for A2A and A2A v1.0](https://docs.mulesoft.com/gateway/latest/agent-policies#a2a-v1-policies), so teams must match policies to the protocol version each agent uses.
- **Suits:** Salesforce and MuleSoft customers governing agents built across several platforms.

## How to choose an agent gateway

Choose an agent gateway by how callers reach agents, whether agent grants need to live with model and tool grants, and what platform you already run. Start from the situation closest to yours, then test with your own agents.

| Situation | Candidates |
|---|---|
| One set of keys for model, tool, and agent traffic | Bifrost, LiteLLM, agentgateway |
| Client and agent use different A2A transports | Bifrost |
| Callers are MCP clients | ContextForge |
| Discovery and a catalog come first | Jarvis Registry, MCP Gateway and Registry, Gravitee |
| Extending an API platform you already run | Kong AI Gateway, Azure API Management, Gravitee, MuleSoft Agent Fabric |
| No gateway infrastructure to operate on AWS | Amazon Bedrock AgentCore Gateway |

Test with a long-running task as well as a single request. Confirm what happens when a client disconnects, whether a failed request is replayed, and whether one trace links the agent call to the model and tool calls it triggered. The [agent gateway evaluation checklist](https://github.com/ai-gateways/agent-gateway#how-to-evaluate-an-agent-gateway) lists what else to check.

## What this list leaves out

This list covers gateways that meet every test above, so several related products are out of scope. The catalog in this organization lists them.

- **Gateways limited to one vendor's agent runtime.** Products that govern only agents hosted on their own platform do not meet the first test. They are in [Agent Gateways](https://github.com/ai-gateways/awesome-ai-gateways#agent-gateways).
- **Agent runtimes.** Platforms that host and run agents, with A2A as an entry point to those agents, are a different category from a gateway in front of agents that run elsewhere.
- **Gateways without documented A2A support.** Products that govern only model or tool traffic are in [Best AI Gateways](https://github.com/ai-gateways/best-ai-gateways#best-open-source-ai-gateways) and [Best MCP Gateways](https://github.com/ai-gateways/best-mcp-gateways#best-open-source-mcp-gateways).
- **Agent frameworks and SDKs.** Tools for building A2A agents are in the [related lists](https://github.com/ai-gateways/awesome-ai-gateways#related-lists).

## Related guides

- [How the A2A protocol works](https://github.com/ai-gateways/agent-gateway#how-the-a2a-protocol-works): Agent Cards, tasks, and transports
- [Security and access control for agents](https://github.com/ai-gateways/agent-gateway#security-and-access-control-for-agents): why agent, model, and tool grants stay separate
- [Observability for multi-agent workflows](https://github.com/ai-gateways/agent-gateway#observability-for-multi-agent-workflows): following one request across agents
- [Agent gateway vs MCP gateway vs LLM gateway](https://github.com/ai-gateways/agent-gateway#agent-gateway-vs-mcp-gateway-vs-llm-gateway): the three hops a single agent task crosses
- [Best AI Gateways](https://github.com/ai-gateways/best-ai-gateways#best-open-source-ai-gateways): the same selective treatment for AI gateways
- [Best MCP Gateways](https://github.com/ai-gateways/best-mcp-gateways#best-open-source-mcp-gateways): the same selective treatment for MCP gateways
- [Awesome AI Gateways](https://github.com/ai-gateways/awesome-ai-gateways#agent-gateways): the full catalog of agent gateways
