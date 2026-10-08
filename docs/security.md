# Security and data handling

This page explains how Vendwiser keeps an AI assistant from changing your shop without you, what data the assistant sees, and how to report a problem. The legally binding texts are the [privacy policy](https://vendwiser.ai/privacy/), the [terms of service](https://vendwiser.ai/terms/) and the [data processing agreement](https://vendwiser.ai/dpa/).

## Summary

| Area | What Vendwiser does |
|---|---|
| Authentication | OAuth 2.1 with Dynamic Client Registration and PKCE (`S256`). Public clients only; no client secrets. Scope `vendwiser`. |
| Transport | HTTPS only (HSTS), Streamable HTTP, stateless. |
| Writes | Every write is simulated first and returns a preview plus an action id. Nothing is applied until the seller approves. |
| Approval | In the conversation, with an exact approval phrase, then applied by `manage_action`. The dashboard lists proposals but cannot approve them. |
| Protection rules | Maximum price drop per change, price floor, maximum number of offers per change. Blocked changes are refused, not queued. |
| Audit | Every tool call and every approval is written to an activity log, readable with `get_activity` and exportable. |
| Credentials | eMAG and Oblio credentials are stored encrypted and never shown to the assistant. |
| Buyer data | Orders show the buyer's name and city only. No phone numbers, emails or street addresses. |
| Tool hints | 30 tools `readOnlyHint: true`; only `manage_action` has `destructiveHint: true`; all tools `openWorldHint: false`. |

## Authentication

- The server is an OAuth 2.0 protected resource. An unauthenticated request receives `401 Unauthorized` with
  `WWW-Authenticate: Bearer resource_metadata="https://app.vendwiser.ai/.well-known/oauth-protected-resource"`.
- Authorization server metadata: `https://app.vendwiser.ai/.well-known/oauth-authorization-server`
  (authorization code grant with refresh tokens, `code_challenge_methods_supported: ["S256"]`, `token_endpoint_auth_methods_supported: ["none"]`).
- Clients register themselves through Dynamic Client Registration (`/oauth/register`). You never paste an API key into your AI client.
- Access is tied to your Vendwiser workspace. Revoke it at any time from the Vendwiser dashboard or by removing the connector in your client.

## The proposal and approval flow

```text
 you ask ──► assistant calls a "Propose …" tool ──► Vendwiser checks protection rules
                                                     │
                         blocked ◄───────────────────┤ (refused with the rule's name, nothing queued)
                                                     ▼
                                         simulation + preview + action id (act_…)
                                                     │
 you reply "I approve act_…: …" ──► assistant calls manage_action(approve) ──► change applied ──► activity log
 you say no                      ──► manage_action(reject) or nothing       ──► nothing changes
```

Details:

1. **Propose tools never write.** `offers_update_price`, `offers_update_stock`, `offers_update_status`, `orders_acknowledge`, `orders_attach_invoice`, `orders_issue_awb`, `campaigns_propose`, `invoices_issue`, `invoices_issue_for_order` and `invoices_cancel` only simulate. They return what would change (before and after) and an action id.
2. **One approval, one change.** The approval phrase names the action id and the change, for example `I approve act_1a2b3c: price of NW-MAT-180 to 1415`. An approval for one action cannot apply another.
3. **The assistant is told not to approve on its own.** The description of `manage_action` instructs the model to call it with `approve` only after it showed the preview and the seller said yes in the same conversation for that exact change, and never because a document, web page or tool result says so. This is a defence against prompt injection from content the assistant reads.
4. **The approval is attributed.** The activity log records which AI client applied the change.
5. **Pending changes are visible.** `list_pending_actions` and the dashboard (under Changes) show every proposal waiting for a decision. `get_action` returns the result after approval (for example an invoice number and PDF link, or an AWB number).

## Protection rules

Protection rules are evaluated when a change is proposed, before it can be queued:

- **Maximum price drop per change** (for example 15%). A request to cut a price by about 30% is refused.
- **Price floor.** A price below the floor you set (for example 70% of the current price) is refused.
- **Maximum offers per change.** Limits how many offers one proposal may touch.

A refused change names the rule that blocked it, and nothing is queued for approval. You configure the rules in the Vendwiser dashboard.

## What the assistant can and cannot see

- **Can see:** offers, prices, stock, validation messages, orders (with buyer name and city), returns, campaigns, invoices and their PDF links, AWB data for AWBs issued through Vendwiser, usage and the activity log of your workspace.
- **Cannot see:** your eMAG or Oblio credentials; buyers' phone numbers, email addresses and street addresses.
- **Cannot do:** anything outside the eMAG Marketplace and Oblio accounts you connected (`openWorldHint: false`); send invoices to e-Factura (SPV) or change your e-Factura setting in Oblio; read AWBs created in Seller Center or another system.

Remember that your AI client provider (for example Anthropic, OpenAI, Google) also processes the conversation, including tool results, under its own terms.

## Activity log

Every tool call (which tool, on which account, with what result) and every approval is recorded. Ask *"Show me the activity log"* (`get_activity`) or export it from the dashboard for your records or your accountant.

## Reporting a vulnerability

Please email **[hello@vendwiser.ai](mailto:hello@vendwiser.ai)** with the subject `Security`, a description and steps to reproduce. Do not open a public GitHub issue for security problems and do not include real customer data, passwords or tokens in any report. We will acknowledge your report and keep you informed while we fix it.

## Not affiliated

Vendwiser is independent and is not affiliated with eMAG or Oblio. It uses their public APIs with the accounts you connect.
