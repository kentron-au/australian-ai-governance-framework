# Australian AI Governance Framework

Australian AI governance framework, built around the legislation a specific organisation's use of AI actually triggers.

This repository contains no source code. The service is closed-source and hosted at [aiframework.com.au](https://aiframework.com.au). This repo exists as the public metadata pointer for MCP directory listings, and as the connector's public documentation.

---

## What it does

Profiles an organisation by industry, size, turnover, states of operation, AI use cases and data types, then identifies the Commonwealth and state instruments that apply to that profile specifically:

- **Privacy Act 1988**, including the Notifiable Data Breaches scheme
- **APP 1.7** — the automated decision-making disclosure required in covered businesses' privacy policies from **10 December 2026**
- Sector-specific regimes triggered by the profile, such as the Heavy Vehicle National Law, the Fair Work Act 2009 or the Security of Critical Infrastructure Act 2018

The mapping is tiered rather than a flat list. Instruments return under **Applies**, **May apply depending on facts you hold**, or **Applicability turns on your own status** — because the questionnaire captures a profile, not an organisation's full circumstances, and several thresholds turn on facts it does not ask for. The Privacy Act small business exemption is the clearest case: at $3 million turnover or less, whether the Act applies depends on which of the ss 6D and 6E categories the organisation falls into.

A tier with nothing in it is left out, so a given document may carry two of these headings rather than three.

Around that mapping the framework is structured on the six essential practices (AI6) in the National AI Centre's **Guidance for AI Adoption**, October 2025: Accountability, Impact Assessment, Risk Management, Transparency & Information Sharing, Testing & Monitoring, and Human Oversight.

The legislative mapping is specific to the profile supplied. It is not something a general answer reliably produces for an Australian organisation.

---

## Endpoint

| | |
|---|---|
| Website | https://aiframework.com.au |
| MCP endpoint | https://aiframework.com.au/api/mcp.php |
| Transport | Streamable HTTP — stateless JSON-RPC 2.0 |
| Authentication | None |
| Server card | https://aiframework.com.au/.well-known/mcp/server-card.json |

There is no authentication and nothing to provision. No API key, no OAuth flow, no account. Any MCP-compatible client can connect and call all three tools immediately.

---

## Installation

This is a remote, hosted MCP server. No local installation is required.

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

**Any other MCP client** — point it at the endpoint over Streamable HTTP and leave authentication unset.

If you have connected before and the tool names or the questionnaire's options look wrong, remove the connector and add it again. Tool lists cache client-side: the tools were renamed in August 2026, and the questionnaire's options change as the legislative register grows.

---

## Tools

| Tool | Purpose |
|---|---|
| `start_australian_ai_governance_framework` | Returns a session ID and the profiling questionnaire |
| `submit_ai_governance_profile` | Takes the organisation profile and contact details |
| `get_ai_governance_framework` | Returns the free preview, a `view_url` and a `purchase_url` |

### 1. `start_australian_ai_governance_framework`

No parameters. Returns a session ID and a seven-question questionnaire: industry, organisation size, states and territories of operation, annual turnover band, AI use cases, data types, and an optional free-text description of the organisation.

### 2. `submit_ai_governance_profile`

Takes `session_id`, `answers` and `contact`.

Six `answers` fields are required — `industry`, `orgSize`, `jurisdiction`, `turnover`, `aiUseCases`, `dataTypes`. `additionalNotes` is optional. Values are validated against the option strings the questionnaire returns, so pass them through unaltered: `jurisdiction` takes full state names rather than abbreviations, and punctuation in option strings is significant.

`contact` requires `name`, `email` and `organisation`, and the server validates the email address. **The organisation name is printed as the document's "Prepared for" heading and appears throughout the generated text**, so it should be the organisation the framework is for.

Returns the session ID and a status of `profile_saved`. It does not return the framework.

Safe to call again on the same session: the profile is overwritten rather than duplicated, and the contact record is keyed on the email address. Re-submitting discards any framework already generated against that session, so call it again only to correct an answer.

### 3. `get_ai_governance_framework`

Takes `session_id`. Returns the free preview, a `view_url` and a `purchase_url`.

The document arrives in the structuredContent field `framework_markdown`, and in the text content block for clients that read those. It is meant to be shown to the user in full as Markdown — it is the thing they asked for, not a payload to summarise.

**Generation takes around 30 seconds.** That is the model producing the document, and it happens on this call. A client with a short tool timeout will give up before the endpoint answers.

---

## Free preview and the full framework

| | Free preview | Full framework |
|---|---|---|
| Price | Nothing | **$88 AUD including GST**, one-time |
| Legislative mapping — *Applies*, and *Applicability turns on your own status* | Complete, for the profile supplied | Complete, for the profile supplied |
| Legislative mapping — *May apply depending on facts you hold* | Part | Complete |
| Practice 1 — Accountability (Decide who is accountable) | Included | Included |
| Practice 2 — Impact Assessment (Understand impacts and plan accordingly) | Included | Included |
| Practice 3 — Risk Management (Measure and manage risks) | — | Included |
| Practice 4 — Transparency & Information Sharing (Share essential information) | — | Included |
| Practice 5 — Testing & Monitoring (Test and monitor) | — | Included |
| Practice 6 — Human Oversight (Maintain human control) | — | Included |

The name before each bracket is the heading used in the document; the name inside is the National AI Centre's own wording for the same practice.

Instruments under **Applies**, and under **Applicability turns on your own status**, are never held back — both print in full in the free preview, Commonwealth and state alike. One tier is capped: **May apply depending on facts you hold**. Commonwealth and state instruments in that tier each have their own limit, and both limits are higher in the purchased framework. Where instruments are held back, the preview says so; where a profile matches fewer than the limit, nothing is capped and no such note appears.

The preview page is read-only. Printing and saving are part of the purchased framework, which also extends the link from two hours to 18 months.

**There is no payment through this endpoint.** The `purchase_url` reopens the preview with the profile already restored, alongside the purchase option, so the questionnaire is not answered a second time. Payment is by Stripe from there. Card details are never handled by aiframework.com.au.

Two complete purchased frameworks are published in full, unedited, so the paid output can be read without paying: **https://aiframework.com.au/samples/**

---

## Do not send personal information

The profile describes an organisation, not a person. Nothing in the questionnaire asks about individuals, and the framework does not need anything about them.

**This matters more over MCP than on the website.** The optional `additionalNotes` field takes free text, and on the web form the user reads a warning before typing into it. Through the connector an agent fills that field on the user's behalf and never sees the warning. If you are building or configuring a client, carry this through:

> Describe the organisation, not individuals. Do not include names, contact details, health or other sensitive information, client details, or anything commercially confidential.

Whatever goes into `additionalNotes` is sent to Anthropic's API for generation and stored with the session.

---

## What happens to what you send

| | |
|---|---|
| Profile selections, organisation name, `additionalNotes` | Sent to Anthropic's API in the United States for generation. Anthropic does not use API data to train its models |
| Contact name and email | Stored as a contact record. **Not** sent to Anthropic |
| Card details | Handled by Stripe. Never transmitted to, seen by, or stored by this site |
| Website and database | Hosted in Australia, by VentraIP Australia |
| Analytics | Self-hosted Matomo on Australian infrastructure. No advertising networks, no cross-site tracking |

Served over HTTPS throughout. API and database credentials sit outside the web root and are never present in a browser. The endpoint applies an IP-based rate limit.

Full detail, including overseas disclosure and the Notifiable Data Breaches position, is in the [privacy policy](https://aiframework.com.au/privacy).

---

## Links and retention

- **Preview links expire two hours after the profile is created** — both the `view_url` and the `purchase_url`. Starting again takes about a minute; the questions are the same seven.
- **A purchased framework stays at its link for 18 months**, then is deleted. That link is how a customer returns to the document, so it is worth saving.
- **An unpaid session is deleted about two hours after it is created**, when the links stop working.
- **Contact records are kept for 18 months** from creation, then deleted. Earlier deletion can be requested.
- A framework reflects the legislative position and the profile as at the date it was generated. It is not updated afterwards.

---

## Troubleshooting

| What you see | What it means |
|---|---|
| `get_ai_governance_framework` times out | Generation takes around 30 seconds. Raise the client's tool timeout above that and call again with the same `session_id` — the profile is still there |
| "Session not found" | Free preview sessions are removed two hours after creation. Call `start_australian_ai_governance_framework` to begin again. Purchased frameworks are not affected and remain available for 18 months |
| A validation error on `submit_ai_governance_profile` | An option string does not match, or the email address is invalid. Pass the questionnaire's own strings through unaltered — `jurisdiction` takes full state names |
| The framework does not reflect the profile submitted | Call `submit_ai_governance_profile` again on the same session to correct the answers, then `get_ai_governance_framework`. If it still does not match, get in touch — it will be regenerated or refunded |
| Tool names not recognised, old names appear, or an option the questionnaire returned is rejected as invalid | Client tool lists cache. The tools were renamed in August 2026, and the questionnaire's options change as the register grows, so a cached schema can reject a current option. Remove the connector and add it again |
| A request is throttled | The endpoint rate-limits by IP. Wait and retry, or get in touch |
| An instrument you expected is missing | The mapping returns legislative instruments. Regulator guidance and standards are out of scope, and coverage reflects the position at the date of generation |

---

## Scope

Output is a framework presentation only — **not advice, not a recommendation, not compliance guidance.** Every framework carries a disclaimer to that effect. The output is not reviewed by a human before delivery.

It does not produce AI application inventories, staff training plans, internal policies, role assignments or governance maturity ratings. Those require knowledge only the organisation holds. What the framework provides is the structure those documents sit within.

No research is performed at generation time. Australian jurisdictions only.

---

## Support, data requests and complaints

One address for all of it: **contact@aiframework.com.au**

- **A problem with a framework** — if it does not arrive, arrives incomplete, or does not reflect the profile submitted, quote the link or the email address used. It will be regenerated or refunded, within 30 days of purchase per the [terms of use](https://aiframework.com.au/terms).
- **A copy of your data, a correction, or deletion** — response within 30 days per the privacy policy. Deletion is actioned unless a law requires the record to be kept.
- **A privacy complaint** — acknowledged and answered within 30 days. If the answer is unsatisfactory, the Office of the Australian Information Commissioner takes complaints at [oaic.gov.au](https://www.oaic.gov.au) or 1300 363 992.
- **A security issue** — same address, and please report it before disclosing publicly.

Operated by **Kentron Pty Ltd** · ABN 31 123 944 927 · New South Wales, Australia. Postal address for written enquiries is in the [privacy policy](https://aiframework.com.au/privacy).

---

## Licence

See LICENSE for this repository's licence. It applies to the repository content only, not to the aiframework.com.au service.
