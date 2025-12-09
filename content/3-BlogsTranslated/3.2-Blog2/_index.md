---
title: "Blog 2"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Flexibility to Framework: Building MCP Servers with Controlled Tool Orchestration

Author: Kevon Mayers
Published: August 13, 2025
Source: [Flexibility to Framework: Building MCP Servers with Controlled Tool Orchestration | AWS DevOps & Developer Productivity Blog](https://aws.amazon.com/blogs/devops/flexibility-to-framework-building-mcp-servers-with-controlled-tool-orchestration/)

[MCP (Model Context Protocol)](https://modelcontextprotocol.io/specification/2025-03-26) is a protocol designed to standardize interactions with generative AI models, making it easier to build and operate AI applications. The protocol provides a consistent way to pass context between an application and a model regardless of where or how the model is deployed. MCP helps close the gap between model deployment and application development by offering a unified interface for model interactions.

While MCP provides flexibility in tool choice, there are important challenges when you need to enforce an order of tool usage. In this article, I describe how I designed this capability and implemented it in the [AWS Cloud Control API (CCAPI) MCP server.](https://awslabs.github.io/mcp/servers/ccapi-mcp-server/)

---

## The challenge — enforcing tool order in MCP

When you think about MCP, you naturally imagine flexibility. One of the primary reasons to use an MCP server is to let a large language model (LLM), via an agent, access a set of tools such as reading from a database, sending an email, or other helper functions. However, the MCP framework does not provide an intrinsic mechanism to force the order in which tools are called.

For example: consider two tools, `fetch_weather_data()` and `send_email()`. You want to ensure the email includes current weather — meaning `fetch_weather_data()` must run before `send_email()`. Or consider `getOrderId()` and `getOrderDetail()`, where you must obtain the OrderId before fetching detailed order information. Because MCP currently lacks a way to define tool-order preferences, enforcing such sequential relationships is difficult.

MCP tools are designed as independent functions the LLM can call when needed. There is no concept of a workflow or sequence inside the MCP framework itself. Each tool invocation is treated as a separate task with no built-in knowledge of what happened before or what will happen next. As a result, by default an LLM may call tools in any order, ignoring the logical sequence you might expect.

While LLMs are excellent at flexible decision-making, certain use cases like infrastructure management require strict ordering. This raises the question for MCP server developers: how do you preserve LLM flexibility while enforcing the required order of critical tools?

When you think about Infrastructure as Code (IaC), you expect repeatability, consistency, versioning, and CI/CD integration. CI/CD pipelines follow a defined sequence:

- Pull request is created
- CI/CD pipeline triggers
- A series of steps run (linting, security checks, unit tests, end-to-end tests, etc.)
- If any step fails, the pipeline stops

The non-deterministic nature of AI complicates this: generative AI is not deterministic — the same prompt might not always produce the same result. If outputs deviate too much from expectations, that's considered a hallucination. So how do you guide an LLM to do what you want? Let's look at how the CCAPI MCP server addresses this.

---

## Understanding tool discovery and initialization in MCP

Before diving into the solution, it's important to understand how an MCP server communicates available tools to AI agents. During initialization, MCP defines lifecycle stages where capabilities and tools are discovered.

The MCP context model defines a structured lifecycle for the client-server connection that negotiates capabilities and manages state. The stages include:

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog2.png" style="width: 100%;" />
</div>

- Initialization: negotiate capabilities and protocol versions
- Operation: normal protocol communication
- Shutdown: gracefully close the connection

Initialization establishes compatibility and shares implementation details. This is when an AI agent learns about available tools via schema definitions and receives usage guidance. Initialization is critical because tools are discovered at this stage. The client sends protocol version information, capability declarations, and deployment details during initialization.

For example, a tool like an Amazon Q CLI receives information about the MCP server version, available tools, and usage guidance via schemas sent by the server.

(See the MCP lifecycle documentation for more details: https://modelcontextprotocol.io/specification/2025-06-18/basic/lifecycle.)

---

## Solution — token-based tool orchestration: a new pattern for MCP agents

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog2.1.png" style="width: 100%;" />
</div>

MCP faces a challenge: tools cannot directly talk to each other to enforce an execution order. The CCAPI MCP server solves this by using a token messenger pattern, where the server issues and controls verification tokens, and the AI agent (the MCP client) passes those tokens between tool invocations.

---

## Key implementation details

- **Function enhancement**

  A `@mcp.tool()` decorator turns each function into an enhanced entity. It wraps the function with a schema that defines required inputs and validation rules while preserving the docstring. Each function explicitly exposes its requirements and raises clear errors if dependencies are not met.

- **Dependency discovery**

  During MCP initialization the AI agent receives a full map of tools and their schemas from the MCP server. The LLM (acting inside the agent) uses these schemas to infer dependencies through parameter descriptions and required inputs.

  For example, if a tool requires a parameter described as "Result from `get_aws_session_info()`" and declares `security_scan_token` as a required input, the LLM will understand that it needs valid tokens before continuing. The combination of textual descriptions and clear required inputs enables the agent to execute a chain like `get_aws_session_info()` → `generate_infrastructure_code()` → `run_checkov()` → `create_resource()`.

- **Token validation control**

  The server issues and controls all workflow tokens via a server-side store (`_workflow_store`). Each tool in the workflow emits a security token that is stored on the server with associated metadata.

  The AI agent holds tokens in its chat context and passes them between tool calls. Each token used by the agent must be validated against the server-side store. Tokens are short-lived, kept in memory, actively managed, and removed after use to preserve freshness. Any leftover tokens are cleared when the server process shuts down or restarts.

  If a token does not exist in the store (because it is invalid or was already consumed), the operation immediately fails with an error. This applies to all token types and ensures that the agent cannot fabricate or modify tokens.

  As the workflow progresses, a tool consumes existing tokens and produces new tokens. For example, when `explain()` receives a `properties_token`, it validates that the token exists and matches the `_workflow_store`, then consumes it and emits a new `explained_properties_token`. This approach forms a cryptographic-like chain of operations that enforces workflow order (generate → scan → create) with server-side verification at each step.

  The result is a predictable workflow system with strong security controls — tokens must be issued by the server and validated against the server store at every step — which enforces tool ordering and data integrity in infrastructure operations. This approach enables reliable workflow execution within the constraints of the current FastMCP framework.

  While explicit dependency declarations like `@mcp.tool(depends_on=["run_checkov"])` (as discussed in a GitHub issue) may be ideal in future versions, the token approach combining descriptive parameter names and explicit validation currently provides a stable ordering mechanism the LLM will follow.

## Potential limitations and mitigations

- **Session management**

  When an agent session terminates or refreshes, any in-progress workflows must restart. This is by design — tokens are short-lived and bound to a specific workflow chain. AWS credentials naturally expire after a few hours as part of standard security policies, which provides a natural boundary for workflow sessions.

- **Concurrent workflows**

  Each agent interaction is isolated, which is appropriate to maintain security boundaries between workflows. While this means each session starts fresh, it guarantees clear separation between independent infrastructure operations.

- **Implementation options**

  Organizations that require long-lived workflow state can use a traditional database to persist session state across restarts. However, because tokens are designed as short-lived security controls, many deployments can rely on in-memory storage with the natural session boundary.

The token messenger pattern provides a robust foundation for secure workflow coordination, using ephemeral tokens to guarantee tool ordering and data integrity when operating on infrastructure.

---

## The future of MCP

While the solution works, it also suggests future directions for MCP. I've seen multiple updates to the framework recently, and it's encouraging to see community activity. With agentic AI overall, there's evidence that platforms can become more deterministic (as emphasized by "hooks" in Claude Code). According to the docs, “Hooks provide deterministic control over Claude Code behavior, ensuring some actions always occur rather than relying on the LLM to choose.” For IaC and other deterministic technologies we want to pair with AI, that determinism is essential for enterprise adoption.

---

## Conclusion

The Model Context Protocol (MCP) journey and the emerging space of AI-driven infrastructure management continue to evolve, bringing both opportunities and challenges to cloud computing and AI. Current approaches like prompt loading and parameter-driven dependencies have helped address early ordering and security concerns, showing MCP can be used effectively in enterprise applications.

While the present implementation using workflow tokens and validation checks provides a workable solution, we continue to explore ways to enhance the protocol. If you’re interested in contributing to MCP’s evolution, check the dependency management improvement proposals in the modelcontextprotocol GitHub organization and the FastMCP repository.

If you want to explore the AWS MCP Cloud Control API server mentioned in the article, see the documentation and GitHub repos. If you’d like hands-on practice with it and other MCP servers, try the linked AWS workshop. Happy coding!

---

## About the author

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog2.2.png" style="width: 100%;" />
</div>
