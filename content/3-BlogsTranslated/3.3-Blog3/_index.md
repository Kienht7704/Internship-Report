---
title: "Blog 3"
date: "2025-09-09T19:53:52+07:00"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Introducing AWS Cloud Control API MCP Server: Natural Language Infrastructure Management on AWS

Author: Kevon Mayers and Brian Terry
Published: August 13, 2025
Source: [Introducing AWS Cloud Control API MCP Server: Natural Language Infrastructure Management on AWS | AWS DevOps & Developer Productivity Blog](https://aws.amazon.com/blogs/devops/introducing-aws-cloud-control-api-mcp-server-natural-language-infrastructure-management-on-aws/)

Today we’re announcing the [AWS Cloud Control API (CCAPI) MCP Server](https://awslabs.github.io/mcp/servers/ccapi-mcp-server/). This MCP server transforms AWS infrastructure management by allowing developers to create, read, update, delete, and list resources using natural language commands. The project is part of [awslabs/mcp](https://github.com/awslabs/mcp) — an experimental effort to bridge conversational intent and infrastructure operations on AWS.

The CCAPI MCP Server is powered by the [AWS Cloud Control API](https://aws.amazon.com/cloudcontrolapi/) — a standardized API that performs CRUDL (Create/Read/Update/Delete/List) operations across AWS and third-party resources through a single access point.

---

## Key features

- Uses AWS Cloud Control API to perform CRUDL operations for over 1,200 AWS resource types

- Enables LLM-based agents and developers to manage infrastructure via natural language

- Optionally exports Infrastructure as Code (IaC) templates for created resources so you can integrate with existing CI/CD pipelines

- Integrates with the AWS Pricing API to provide cost estimates for the infrastructure it will create

- Applies security best practices automatically via [Checkov](https://www.checkov.io/)

---

## Why use the CCAPI MCP Server?

- Simpler infrastructure management: no more wrestling with complex config templates or long manuals

- Increased developer productivity: focus on intent, not the exact configuration syntax

- Lower onboarding friction: new team members can perform common infra tasks with natural language

- LLM integration: a natural companion for AI-assisted development workflows

The MCP Server lets developers manage cloud resources with conversational commands such as:
- “Can you create a new S3 bucket for me?”
- “List all my EC2 instances and tell me which ones are not t2.large”

This reduces configuration overhead and makes intent-to-infrastructure mappings more direct.

---

## How to create and manage cloud resources

### Prerequisites

- A package manager (`pip`) installed

- Python 3.x

- AWS credentials configured with appropriate permissions. The MCP server supports multiple ways to provide credentials — see the MCP documentation for details. We recommend using dynamic credentials (for example, via SSO). For AWS credential configuration guidance, consult the AWS CLI documentation.

- An MCP host application that can run both MCP clients and MCP servers (for example: Amazon Q Developer, Claude Desktop, Cursor, etc.). To follow the examples in this post, install Amazon Q Developer for CLI as described in its installation guide.

---

## Integration with developer tools

To start using the CCAPI MCP Server, configure the server settings (typically in a file named `mcp.json`). This post focuses on using the CCAPI MCP Server with [Amazon Q Developer](https://aws.amazon.com/q/developer/). Note that other MCP host apps may use different locations for their MCP config files.

Configuration locations include:

- Global configuration: `~/.aws/amazon/mcp.json` — applies to all workspaces
- Workspace configuration: `.amazonq/mcp.json` — applies to the current workspace

See the Amazon Q Developer User Guide for more details.

---

## Configuration file structure

The MCP config uses JSON. Example configuration screenshots are shown below.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.png" style="width: 100%;" />
</div>

Example `mcp.json` for the CCAPI MCP Server:

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.1.png" style="width: 100%;" />
</div>

---

## Important configuration notes

Ensure you configure AWS credentials properly for the MCP server. The server uses those credentials when calling Cloud Control API to perform CRUDL operations in your AWS account. The server supports various credential sources such as AWS profiles, environment variables, SSO tokens, etc. See `aws_client.py` in the project for examples and refer to named profile documentation for more details.

---

## Read-only mode

If you want the MCP server to avoid making mutating actions (Create/Update/Delete), run it with the `--readonly` flag.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.2.png" style="width: 100%;" />
</div>

---

## Security considerations

- Ensure the IAM credentials used have the permissions required by the Cloud Control API (List, Get, Create, Update, Delete). Consult the AWS CCAPI docs for details.
- Follow the principle of least privilege when configuring IAM permissions.
- Enable AWS CloudTrail to record and audit operations.
- Consider running the server in read-only mode (`--readonly`) for safer operation when appropriate.

---

## Example: Create an S3 bucket encrypted with KMS

**Important:** Ensure you’ve satisfied the prerequisites before running the example.

Once `mcp.json` is configured correctly, try the example. In your terminal, run `q chat` to start Amazon Q in the CLI.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.3.png" style="width: 100%;" />
</div>

That command starts background MCP servers, allowing Q Chat to use them even while startup continues. If the MCP servers are still loading, your prompts may be processed without using MCP servers. To check server status, run `/mcp`.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.4.png" style="width: 100%;" />
</div>

When servers are ready, try a sample prompt. Tell Amazon Q: `Create an S3 bucket with versioning and encrypt it using a new KMS key`.

Amazon Q will automatically perform these steps:
- Read your current environment variables
- Use them to retrieve the current AWS session info
- Generate infrastructure code describing the requested resources
- Explain the generated code
- Run a security scan on the generated code (if enabled)
- Explain the scan results
- Validate the configuration against Cloud Control API schemas (based on CloudFormation Resource Provider Schemas) and IAM policies
- Create resources directly via Cloud Control API

Note: CloudFormation schemas are used for resource structure reference only — CCAPI MCP Server uses Cloud Control API for resource management rather than CloudFormation itself.

Before taking destructive actions, Amazon Q will summarize the steps and request confirmation (type `y` to proceed when prompted).

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.5.png" style="width: 100%;" />
</div>

Next, Amazon Q may call `get_aws_session_info()` to fetch session details according to values in your MCP config (e.g., `~/.aws/amazon/mcp.json`) and environment variables.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.6.png" style="width: 100%;" />
</div>

The tool will then display the AWS account ID and region it will use, generate infrastructure code for the KMS key (and related properties), and send the properties to Cloud Control API for validation and Checkov scanning. The KMS key will be configured with recommended security practices and a key policy constrained to the current account.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.7.png" style="width: 100%;" />
</div>

After the code is generated, Amazon Q will use `explain()` to describe the generated infrastructure code. The MCP server automatically adds default tags to managed resources: `MANAGED_BY`, `MCP_SERVER_SOURCE_CODE`, `MCP_SERVER_VERSION`. These tags help you identify resources managed by the MCP Server; you can customize or disable them, but keeping identifying tags is recommended for visibility.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.8.png" style="width: 100%;" />
</div>

Amazon Q will then run `run_checkov()` to perform a security scan on the generated code (this runs automatically if `SECURITY_SCANNING` is enabled in your server config).

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.9.png" style="width: 100%;" />
</div>

If Checkov finds issues, Amazon Q will explain the findings and suggest remediations. Passing checks proceed automatically with a short summary; request more detail if needed.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.10.png" style="width: 100%;" />
</div>

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.11.png" style="width: 100%;" />
</div>

Next, Amazon Q uses `create_resource()` to create resources via Cloud Control API, then polls `get_resource_request_status()` using the request token to track progress.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.12.png" style="width: 100%;" />
</div>

Amazon Q will continue using CCAPI MCP Server tools until the S3 bucket and KMS key are created, then produce a summary of the workflow.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.13.png" style="width: 100%;" />
</div>

Try asking Amazon Q to perform a potentially risky change (for example, make an S3 bucket publicly accessible). Amazon Q will warn that the change violates best practices, explain why, and request confirmation before proceeding.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.14.png" style="width: 100%;" />
</div>

The CCAPI MCP Server integrates with AWS Pricing API so you can request cost estimates for resources it plans to create.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.15.png" style="width: 100%;" />
</div>

You can also ask Amazon Q to generate a CloudFormation template of the resources it created so you can save a backup or redeploy later. The `create_template()` tool has these defaults:
- Outputs YAML by default (JSON option available)
- Sets DeletionPolicy = RETAIN
- Sets UpdateReplacePolicy = RETAIN
- Allows optional parameters such as template ID, file location, and region

Refer to the tool’s source code for more details.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.16.png" style="width: 100%;" />
</div>

Try another risky operation, like deleting all resources in an account — the security checks will block the attempt and propose safer alternatives.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.17.png" style="width: 100%;" />
</div>

Finally, ask Amazon Q to delete the resources it previously created. The toolchain will:
- Use `get_resource()` to find the resources it created
- Use `explain()` to describe the planned changes
- Use `delete_resource()` to remove them

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.18.png" style="width: 100%;" />
</div>

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.19.png" style="width: 100%;" />
</div>

After successful deletion, Amazon Q shows a final summary.

<div style="text-align: center;">
  <img src="/images/3-BlogsTranslated/Blog3.20.png" style="width: 100%;" />
</div>

## Starter prompts

### Sample prompts
- “Create a VPC with private and public subnets”
- “List all my EC2 instances”
- “Create a serverless API for my application”
- “Set up a load-balanced web application”

### Capabilities
- Provision a full networking environment with private and public subnets
- List all running EC2 instances in your account
- Deploy an API Gateway integrated with Lambda (serverless)
- Create a load-balanced web application (ALB) with a target group and instances

## Conclusion

The AWS Cloud Control API MCP Server is an important step forward for AWS infrastructure management, enabling natural-language-driven operations. Whether you’re optimizing operations, experimenting with AI, or onboarding new team members — via Amazon Q Developer CLI or any MCP host (Claude Desktop, Cursor, etc.) — the CCAPI MCP Server and its companion tools provide an intuitive interface to AWS.

<div style="text-align: center;">
<img src="/images/3-BlogsTranslated/Blog2.2.png" style="width: 100%;" />
</div>
