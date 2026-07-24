# AI Governance Framework Generator

Generates tailored AI governance frameworks for Australian businesses, aligned with the six AI6 practices in Australia's Voluntary AI Safety Standard (NAIC Guidance for AI Adoption).

This repository contains no source code. The service is closed-source and hosted at aiframework.com.au. This repo exists solely as a public metadata pointer for MCP directory listings.

- Website: https://aiframework.com.au
- MCP endpoint: https://aiframework.com.au/api/mcp.php
- Transport: Streamable HTTP (stateless JSON-RPC 2.0, no auth)
- Tools: start_session, submit_answers, get_framework

Frameworks, not advice. See the website for full details.

## Installation

This is a remote, hosted MCP server — no local installation is required. Connect any MCP-compatible client directly to the endpoint over Streamable HTTP.

### Claude Desktop / Claude Code

Add to your MCP client configuration:

```json
{
  "mcpServers": {
    "aiframework-governance-generator": {
      "url": "https://aiframework.com.au/api/mcp.php"
    }
  }
}
```

No API key or authentication is required. The endpoint is stateless JSON-RPC 2.0 over HTTP POST.

## Usage

1. Call `start_session` to begin.
2. Call `submit_answers` with your organisation's profile (industry, size, AI use cases, data types).
3. Call `get_framework` to receive a free preview covering the first two of six AI6 practices, with a link to purchase the full six-practice framework at aiframework.com.au.

## Licence

See [LICENSE](LICENSE) for this repository's licence. This applies to the repository content only, not to the aiframework.com.au service itself.
