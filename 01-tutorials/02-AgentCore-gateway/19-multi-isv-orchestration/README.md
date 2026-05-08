<!-- Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. -->
<!-- SPDX-License-Identifier: Apache-2.0 -->

# Amazon Bedrock AgentCore Gateway — Multi-ISV Orchestration (Salesforce + SAP)

This tutorial series demonstrates how to connect multiple ISV SaaS platforms (Salesforce Lightning Platform and AWS for SAP MCP Server) to a single [Amazon Bedrock AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html), enabling cross-system AI agent workflows through one unified endpoint.

## Tutorial Details

| Information | Details |
|:---|:---|
| Tutorial type | Interactive |
| AgentCore components | AgentCore Gateway, AgentCore Identity |
| Agentic Framework | [Strands Agents](https://github.com/strands-agents/sdk-python) |
| Gateway Target types | Integration Provider Template (Salesforce), MCP Server (SAP) |
| Inbound Auth IdP | Amazon Cognito |
| Outbound Auth | CustomOauth2 (Salesforce Connected App), CustomOauth2 (SAP Cognito) |
| LLM model | Anthropic Claude Sonnet 4 |
| Tutorial vertical | Enterprise CRM + ERP |
| Example complexity | Medium |
| SDK used | boto3, requests |

## Tutorials

| # | Notebook | Description |
|---|---|---|
| 1 | [01-salesforce-gateway-target.ipynb](01-salesforce-gateway-target.ipynb) | Add Salesforce Lightning Platform as a Gateway integration target with CustomOauth2 |
| 2 | [02-sap-mcp-server-target.ipynb](02-sap-mcp-server-target.ipynb) | Add AWS for SAP MCP Server as a Gateway MCP target |
| 3 | [03-cross-isv-queries.ipynb](03-cross-isv-queries.ipynb) | Cross-system queries combining Salesforce + SAP through one gateway |

## Architecture

```
                          ┌─────────────────────────────────┐
                          │  Amazon Bedrock AgentCore Gateway │
                          │  (Single MCP endpoint)           │
                          └─────────┬──────────┬────────────┘
                                    │          │
                    ┌───────────────┘          └───────────────┐
                    ▼                                          ▼
    ┌───────────────────────────┐          ┌───────────────────────────┐
    │  Salesforce Lightning      │          │  AWS for SAP MCP Server    │
    │  Platform (Integration     │          │  (MCP Server Target)       │
    │  Provider Template)        │          │                           │
    │                           │          │  ┌─────────────────────┐  │
    │  43 tools: Account, Case, │          │  │ SAP S/4HANA (OData) │  │
    │  Contact, Lead, Opp, ...  │          │  └─────────────────────┘  │
    └───────────────────────────┘          └───────────────────────────┘
```

## Prerequisites

- An AWS account with access to Amazon Bedrock AgentCore
- Python 3.11+
- A Salesforce Developer Edition org with a Connected App configured for OAuth2 `client_credentials` flow
- Access to an AWS for SAP MCP Server deployment (see [documentation](https://docs.aws.amazon.com/mcp-sap/latest/awsforsapmcp/introduction.html))
- AWS CLI configured with appropriate credentials

## Getting Started

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Open the first notebook and follow the steps sequentially:
   ```bash
   jupyter notebook 01-salesforce-gateway-target.ipynb
   ```

3. Each notebook will prompt you for credentials and guide you through the full setup, invocation, and cleanup process.

## Important Notes

- **Salesforce Developer Edition orgs** hibernate after ~24 hours of inactivity. Log into the Salesforce web UI to wake the org before running the notebooks.
- **CustomOauth2** is required for Salesforce Developer Edition orgs. The built-in `SalesforceOauth2` vendor uses `login.salesforce.com` which does not support `client_credentials` on Developer Edition domains.
- **SAP MCP Server** runs in read-only mode by default. Write operations must be explicitly enabled in the SAP MCP Server configuration.

## Disclaimer

This is sample code for demonstration purposes only. Not intended for production use without additional security testing.

## License

This project is licensed under the Apache License 2.0. See the [LICENSE](../../../../LICENSE) file for details.
