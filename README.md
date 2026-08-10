# Australian AI Governance Framework

Australian AI governance framework, built around the legislation a specific organisation's use of AI actually triggers.

This repository contains no source code. The service is closed-source and hosted at [aiframework.com.au](https://aiframework.com.au). This repo exists solely as a public metadata pointer for MCP directory listings.

---

## What it does

Profiles an organisation by industry, size, turnover, state, AI use cases and data types, then identifies the Commonwealth and state instruments that apply to that profile specifically:

- **Privacy Act 1988**, including the Notifiable Data Breaches scheme
- **APP 1.7** — the automated decision-making disclosure required in privacy policies from **10 December 2026**
- Sector-specific regimes triggered by the profile, such as the Heavy Vehicle National Law, the Fair Work Act 2009 or the Security of Critical Infrastructure Act 2018

The framework is structured around the six essential practices (AI6) in the National AI Centre's **Guidance for AI Adoption**, October 2025: Accountability, Impact Assessment, Risk Management, Transparency & Information Sharing, Testing & Monitoring, and Human Oversight.

The legislative mapping is specific to the profile supplied. It is not something a general answer reliably produces for an Australian organisation.

---

## Endpoint

| | |
|---|---|
| Website | https://aiframework.com.au |
| MCP endpoint | https://aiframework.com.au/api/mcp.php |
| Transport | Streamable HTTP — stateless JSON-RPC 2.0 |
| Authentication | None |

---

## Tools

| Tool | Purpose |
|---|---|
| `start_australian_ai_governance_framework` | Returns a session ID and the profiling questionnaire |
| `submit_ai_governance_profile` | Takes the organisation profile and contact details |
| `get_ai_governance_framework` | Returns the free preview, a `view_url` and a `purchase_url` |

---

## Installation

This is a remote, hosted MCP server. No local installation is required — connect any MCP-compatible client directly to the endpoint over Streamable HTTP.

**Claude Desktop / Claude Code**

```json
{
  "mcpServers": {
    "australian-ai-governance-framework": {
      "url": "https://aiframework.com.au/api/mcp.php"
    }
  }
}
```

No API key or authentication is required.

---

## Usage

1. Call `start_australian_ai_governance_framework` to begin. It returns a session ID and the profiling questionnaire.
2. Call `submit_ai_governance_profile` with the organisation's answers and contact details.
3. Call `get_ai_governance_framework` to receive the free preview.

The preview covers the first two of the six AI6 practices — Accountability and Impact Assessment — together with the legislative mapping for the profile supplied. It includes a `view_url` for reading the same document in a browser.

The complete six-practice framework is a one-time **$88 AUD including GST** purchase at [aiframework.com.au](https://aiframework.com.au). There is no payment through this endpoint. The preview returns a `purchase_url` that opens the website with the profile already loaded, so the questionnaire is not answered twice.

Both links expire two hours after the profile is created.

---

## Scope

Output is a framework presentation only — **not advice, not a recommendation, not compliance guidance.** Every framework carries a disclaimer to that effect.

It does not produce AI application inventories, staff training plans, internal policies, role assignments or governance maturity ratings. Those require knowledge only the organisation holds.

---

## Licence

See LICENSE for this repository's licence. It applies to the repository content only, not to the aiframework.com.au service.
