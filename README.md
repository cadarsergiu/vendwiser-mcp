<p align="center">
  <img src="assets/banner-1200x200.png" alt="Vendwiser: run your eMAG and Trendyol shops from your AI assistant" width="100%">
</p>

<h1 align="center">Vendwiser MCP server</h1>

<p align="center">
  <strong>Run your eMAG and Trendyol marketplace shops by asking: offers, product pages, orders, returns, customer questions, invoices, and eMAG shipping labels and campaigns, with your approval on every change.</strong>
</p>

<p align="center">
  <a href="https://vendwiser.ai">Website</a> ·
  <a href="https://vendwiser.ai/docs/">Documentation</a> ·
  <a href="docs/tools.md">All 52 tools</a> ·
  <a href="docs/security.md">Security</a> ·
  <a href="README.ro.md">Română</a>
</p>

[![smithery badge](https://smithery.ai/badge/sergiu/vendwiser)](https://smithery.ai/servers/sergiu/vendwiser)

<!-- Directory badges: uncomment once the listings exist and replace the placeholders.
[![Vendwiser on Glama](https://glama.ai/mcp/connectors/GLAMA_CONNECTOR_ID/badge)](https://glama.ai/mcp/connectors/GLAMA_CONNECTOR_ID)
-->

> **This repository contains documentation and client configuration files only.** The Vendwiser server is a hosted, proprietary service operated by VOLTIVO POWER TRADING SRL. There is nothing to build or run locally: you connect your AI client to the remote endpoint below.

| | |
|---|---|
| **Server URL** | `https://app.vendwiser.ai/mcp` |
| **Transport** | Streamable HTTP (stateless) |
| **Authentication** | OAuth 2.1 with Dynamic Client Registration and PKCE (S256). You sign in to Vendwiser in a browser window; no API keys to copy. |
| **Tools** | 52 (33 read-only, 17 that only *propose* changes, 1 router, 1 that applies approved changes) |
| **Marketplaces** | eMAG Marketplace Romania. Trendyol Romania, Bulgaria, Greece, Czechia, Slovakia and Germany (each storefront connected separately). Oblio for invoicing (optional). |
| **Registry name** | `ai.vendwiser/vendwiser` |

## What it is

Vendwiser connects your **eMAG** and **Trendyol** seller accounts and your **Oblio invoicing account** to the AI assistant you already use (Claude, ChatGPT, Cursor, VS Code, Gemini CLI and any other MCP client), so you can run daily marketplace work in plain language.

Ask which orders are past their handover deadline, why an offer was rejected, which products lost the buy button, which customer questions are still unanswered, or what Trendyol paid you last week. When you want to change something, the assistant prepares the change, shows you a before-and-after preview, and waits for your explicit approval.

## Who it's for

- **eMAG Marketplace sellers in Romania** who want answers and routine changes without clicking through Seller Center.
- **Trendyol sellers** in Romania, Bulgaria, Greece, Czechia, Slovakia and Germany who want orders, product pages, returns and customer questions handled from a conversation.
- **Shop operators and small teams** who invoice eMAG orders in **Oblio** and want invoices and AWBs prepared from the order itself.
- **Agencies and integrators** who manage seller accounts and need an audit trail of every change an AI assistant makes.

You need a Vendwiser account (a free plan is available, see [vendwiser.ai](https://vendwiser.ai) for plans) and an eMAG Marketplace Romania or Trendyol seller account with API access. Each Trendyol storefront is a separate marketplace account: the Free, Starter and Pro plans include one account per marketplace, Scale up to six, which covers every Trendyol country. Oblio is optional.

## Features

- **Catalog:** list offers with price, stock, status, validation state and buy-button position; exact catalog counts; full validation messages; category rules; VAT rates and handling times (eMAG).
- **Product pages (Trendyol):** read a product page and propose changes to the title, description (whole or find-and-replace), attributes and delivery days; propose new products; delete archived variants.
- **Pricing and stock:** propose new prices, stock levels, and turning offers on or off, always as a preview first.
- **Orders:** list, count and inspect orders; find orders past the handover deadline; propose acknowledging new orders; on Trendyol, split an order into packages or cancel items that cannot be supplied.
- **Shipping (AWB, eMAG only):** propose an AWB for an order through your eMAG courier accounts, with weight and parcels suggested from product data; read AWBs issued through Vendwiser.
- **Returns:** list return requests, newest first; on Trendyol, propose accepting or refusing a return.
- **Customer questions (Trendyol):** list the questions buyers asked on product pages and propose an answer; Trendyol reviews it before publishing.
- **Finance (Trendyol):** sales, returns, discounts, coupons, payouts and the commission and shipping invoices Trendyol deducted, exactly as Trendyol reports them.
- **Campaigns (eMAG only):** list campaigns you can join with their dates and rules; propose products for a campaign.
- **Invoicing (Oblio):** list and inspect invoices and proformas; propose issuing an invoice, issuing the invoice for a marketplace order in RON (eMAG Romania, Trendyol Romania; buyer, products, shipping and vouchers taken from the order), or cancelling an invoice.
- **Analysis (eMAG and Trendyol):** catalog health audit, buy-button check with the gap to the winning price, price parity between any two accounts (two Trendyol countries, or eMAG and Trendyol; no currency conversion or VAT normalisation), unshipped-orders report.
- **Safety:** proposals with in-conversation approval, protection rules, and an exportable activity log (see below).

## Safety model

Vendwiser is built so that an AI assistant can **read freely but never change your shop on its own**.

1. **Reads are immediate.** Offers, product pages, stock, prices, orders, returns, customer questions, finance, campaigns, invoices and reports come straight from your accounts.
2. **Every write is a proposal.** Tools that change something (prices, stock, offer status, product pages, new or deleted products, order acknowledgement, order splits and cancellations, return decisions, answers to customer questions, invoices, AWBs, campaign entries) only *simulate* the change. They return a before-and-after preview and an action id such as `act_1a2b3c`. Nothing reaches eMAG, Trendyol or Oblio at this point.
3. **You approve in the conversation.** The assistant shows you an exact approval phrase, for example:

   ```text
   I approve act_1a2b3c: price of NW-MAT-180 to 1415
   ```

   Only after you reply with it does the assistant call `manage_action` to apply that one change. You can also reject it. The approval is recorded with the name of the AI client that applied it.
4. **Protection rules block risky changes before they are even queued**: a maximum price drop per change (for example 15%), a price floor, and a limit on how many offers one change may touch. A blocked change is refused with the name of the rule; nothing is queued.
5. **Everything is logged.** Every tool call and every approval goes into an activity log you can read with `get_activity` and export from the dashboard.

Other protections: marketplace and invoicing credentials are stored encrypted and are never shown to the assistant; buyers' phone numbers, emails and street addresses are not passed to the assistant (orders show only the buyer's name and city). `manage_action` is the only tool marked `destructiveHint: true`, and every tool is `openWorldHint: false`. Some approved changes cannot be undone on the marketplace (deleting Trendyol variants, cancelling order items); their previews say so. More in [docs/security.md](docs/security.md).

## Quick connect

The server URL is the same everywhere: **`https://app.vendwiser.ai/mcp`**. The first time you use it, your client opens a browser window where you sign in to Vendwiser and press **Allow access**.

### Claude (claude.ai and Claude Desktop)

**One click:** [**Add Vendwiser to Claude**](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Vendwiser&connectorUrl=https%3A%2F%2Fapp.vendwiser.ai%2Fmcp)

Or manually: **Settings → Connectors → Add custom connector**, name `Vendwiser`, URL `https://app.vendwiser.ai/mcp`, then **Connect**. On Team and Enterprise plans an owner may need to add the connector for the organization first.

### Claude Code

```bash
claude mcp add --transport http vendwiser https://app.vendwiser.ai/mcp
```

Then run `/mcp` inside Claude Code and choose **Authenticate** for `vendwiser`.

### ChatGPT

1. Go to **Settings → Apps → Advanced settings** and turn on **Developer mode** (available on plans that support custom MCP apps).
2. Choose **Create app**, name it `Vendwiser`, enter `https://app.vendwiser.ai/mcp`, and select **OAuth** as authentication.
3. Sign in to Vendwiser in the window that opens and press **Allow access**.

If Vendwiser is listed in the ChatGPT app directory in your region, you can add it from there instead.

### Cursor

**One click:** [**Add to Cursor**](https://cursor.com/en/install-mcp?name=vendwiser&config=eyJ1cmwiOiJodHRwczovL2FwcC52ZW5kd2lzZXIuYWkvbWNwIn0%3D)

Or add this to `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (project):

```json
{
  "mcpServers": {
    "vendwiser": {
      "url": "https://app.vendwiser.ai/mcp"
    }
  }
}
```

This repository is also a Cursor plugin (`.cursor-plugin/plugin.json`).

### VS Code (GitHub Copilot)

**One click:** [**Install in VS Code**](https://insiders.vscode.dev/redirect/mcp/install?name=vendwiser&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapp.vendwiser.ai%2Fmcp%22%7D)

Or from a terminal:

```bash
code --add-mcp '{"name":"vendwiser","type":"http","url":"https://app.vendwiser.ai/mcp"}'
```

Or run **MCP: Add Server** from the Command Palette, choose **HTTP**, and enter the URL. The equivalent `mcp.json` entry:

```json
{
  "servers": {
    "vendwiser": {
      "type": "http",
      "url": "https://app.vendwiser.ai/mcp"
    }
  }
}
```

### Gemini CLI

Install the extension from this repository:

```bash
gemini extensions install https://github.com/cadarsergiu/vendwiser-mcp
```

Or add only the server:

```bash
gemini mcp add --transport http vendwiser https://app.vendwiser.ai/mcp
```

Start `gemini`, then run `/mcp auth vendwiser` to sign in.

### Cline

Open **MCP Servers → Remote Servers**, name it `vendwiser`, enter `https://app.vendwiser.ai/mcp`, and complete the sign-in. Or add to `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "vendwiser": {
      "type": "streamableHttp",
      "url": "https://app.vendwiser.ai/mcp"
    }
  }
}
```

Agent-readable setup instructions are in [llms-install.md](llms-install.md).

### Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`, then refresh the MCP panel and sign in:

```json
{
  "mcpServers": {
    "vendwiser": {
      "serverUrl": "https://app.vendwiser.ai/mcp"
    }
  }
}
```

### Any other MCP client

| Setting | Value |
|---|---|
| URL | `https://app.vendwiser.ai/mcp` |
| Transport | Streamable HTTP (`streamable-http`), stateless |
| Auth | OAuth 2.1, Dynamic Client Registration, PKCE `S256`, public client (no secret) |
| Protected resource metadata | `https://app.vendwiser.ai/.well-known/oauth-protected-resource` |
| Authorization server metadata | `https://app.vendwiser.ai/.well-known/oauth-authorization-server` |
| Scope | `vendwiser` |

An unauthenticated request returns `401` with a `WWW-Authenticate: Bearer resource_metadata="…"` header, so spec-compliant clients discover the OAuth flow automatically. A ready-made config is in [mcp.json](mcp.json).

For clients that only support local (stdio) servers, the community bridge [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) can proxy the remote endpoint:

```json
{
  "mcpServers": {
    "vendwiser": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://app.vendwiser.ai/mcp"]
    }
  }
}
```

<!-- Optional, confirm before enabling: the server's 401 message says it also accepts an API key sent as a Bearer token.
If you want to document this for clients without OAuth support, describe here where users create the key. -->

## Tools

The last column shows which marketplaces each tool supports (in your client, each tool's "Works on:" line lists the accounts you have actually connected): **eMAG** (Romania), **Trendyol** (Romania, Bulgaria, Greece, Czechia, Slovakia, Germany) or **Oblio**. Legend: 📖 **Read** (`readOnlyHint: true`, returns data) · ✍️ **Propose** (simulates a change and waits for your approval) · 🔀 **Router** (runs another tool by name; behaves exactly like that tool) · ⚠️ **Destructive** (applies or rejects a change you approved; `destructiveHint: true`). All tools have `openWorldHint: false`.

### Getting started and tool discovery

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`start_here`](docs/tools.md#start_here) | Get started | What Vendwiser can do for this seller right now: connected accounts, remaining calls, changes waiting for approval and the available tools. | 📖 Read | Workspace |
| [`search_tools`](docs/tools.md#search_tools) | Find a tool | Find the right Vendwiser tool for a task, described in plain words (for example "issue an invoice" or "orders not shipped"). | 📖 Read | Workspace |
| [`get_tool_schema`](docs/tools.md#get_tool_schema) | Get a tool's inputs | Exact inputs of one or more tools, by name. | 📖 Read | Workspace |
| [`run_tool`](docs/tools.md#run_tool) | Run any tool by name | Run any Vendwiser tool by name with its arguments. | 🔀 Router | Workspace |

### Accounts and usage

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`list_accounts`](docs/tools.md#list_accounts) | List connected accounts | Marketplace accounts connected to this workspace, with what the assistant may do on each. | 📖 Read | Workspace |
| [`connection_health`](docs/tools.md#connection_health) | Check connection health | Test that each connected marketplace account answers. | 📖 Read | Workspace |
| [`get_usage_status`](docs/tools.md#get_usage_status) | Get usage status | Calls used and included this month, extra-call cost, and where the calls went. | 📖 Read | Workspace |

### Approvals and activity log

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`list_pending_actions`](docs/tools.md#list_pending_actions) | List changes waiting for approval | Changes that were simulated and are waiting for a person to approve or reject. | 📖 Read | Workspace |
| [`get_action`](docs/tools.md#get_action) | Get a proposed change | Status and result of one proposed change, by its action_id. | 📖 Read | Workspace |
| [`manage_action`](docs/tools.md#manage_action) | Apply a change the seller approved | Apply or discard a change that is waiting for approval. | ⚠️ Destructive | Workspace |
| [`get_activity`](docs/tools.md#get_activity) | Get activity log | Recent entries from the activity log: which tool ran, on which account, with what result. | 📖 Read | Workspace |

### Catalog and offers

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`offers_list`](docs/tools.md#offers_list) | List offers | List the seller's offers with price, stock, status, validation state and buy-button position. | 📖 Read | eMAG, Trendyol |
| [`offers_summary`](docs/tools.md#offers_summary) | Offers summary | Fast exact counts for the whole catalog: total, active, inactive, rejected, with a rejected update. | 📖 Read | eMAG, Trendyol |
| [`offers_get`](docs/tools.md#offers_get) | Get an offer | Full details of one offer, including every validation message. | 📖 Read | eMAG, Trendyol |
| [`offers_validation_errors`](docs/tools.md#offers_validation_errors) | List rejected offers | Offers the marketplace rejected or blocked, each with the reasons given. | 📖 Read | eMAG, Trendyol |
| [`categories_get`](docs/tools.md#categories_get) | Get a category | A category's rules: mandatory characteristics, allowed values, whether an EAN is required. | 📖 Read | eMAG, Trendyol |
| [`vat_list`](docs/tools.md#vat_list) | List VAT rates | VAT rates available on this account, with their ids. | 📖 Read | eMAG |
| [`handling_time_list`](docs/tools.md#handling_time_list) | List handling times | Handling-time values the marketplace accepts. | 📖 Read | eMAG |
| [`offers_update_price`](docs/tools.md#offers_update_price) | Propose price changes | Propose new sale prices. | ✍️ Propose | eMAG, Trendyol |
| [`offers_update_stock`](docs/tools.md#offers_update_stock) | Propose stock changes | Propose new stock values. | ✍️ Propose | eMAG, Trendyol |
| [`offers_update_status`](docs/tools.md#offers_update_status) | Propose turning offers on or off | Propose activating or deactivating offers. | ✍️ Propose | eMAG, Trendyol |
| [`offers_content_get`](docs/tools.md#offers_content_get) | Get a product page | The content of one product page: title, description, images, attributes, which attributes are not filled in, delivery days. | 📖 Read | Trendyol |
| [`offers_update_content`](docs/tools.md#offers_update_content) | Propose product page changes | Propose changes to titles, descriptions (whole or find-and-replace), attributes and delivery days; Trendyol reviews them before showing them. | ✍️ Propose | Trendyol |
| [`offers_create`](docs/tools.md#offers_create) | Propose new products | Propose new products (variants share a product_main_id); Trendyol reviews each product before it goes on sale. | ✍️ Propose | Trendyol |
| [`offers_delete`](docs/tools.md#offers_delete) | Propose deleting archived variants | Propose deleting variants archived for more than a day. Cannot be undone. | ✍️ Propose | Trendyol |

### Orders and shipping

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`orders_list`](docs/tools.md#orders_list) | List orders | List orders in a date window (default: last 7 days, at most 31 days). | 📖 Read | eMAG, Trendyol |
| [`orders_get`](docs/tools.md#orders_get) | Get an order | One order with its lines, AWB and invoice state. | 📖 Read | eMAG, Trendyol |
| [`orders_count`](docs/tools.md#orders_count) | Count orders | Number of orders, optionally in one status. | 📖 Read | eMAG, Trendyol |
| [`couriers_list`](docs/tools.md#couriers_list) | List courier accounts | Courier accounts configured on the seller account. | 📖 Read | eMAG |
| [`awb_get`](docs/tools.md#awb_get) | Get an AWB issued through Vendwiser | One AWB by its reservation id (returned when the AWB was issued through Vendwiser): courier, AWB numbers, status, cash on delivery. | 📖 Read | eMAG |
| [`orders_acknowledge`](docs/tools.md#orders_acknowledge) | Propose acknowledging orders | Propose acknowledging new orders (moves them to "in progress"). | ✍️ Propose | eMAG, Trendyol |
| [`orders_attach_invoice`](docs/tools.md#orders_attach_invoice) | Propose attaching an invoice | Propose attaching an invoice PDF (by public URL) to an order. | ✍️ Propose | eMAG, Trendyol |
| [`orders_issue_awb`](docs/tools.md#orders_issue_awb) | Propose an AWB for an order | Propose an AWB (shipping label) for one order through the marketplace's courier accounts. eMAG only. | ✍️ Propose | eMAG |
| [`orders_split`](docs/tools.md#orders_split) | Propose splitting an order into packages | Propose sending part of an order in separate packages. | ✍️ Propose | Trendyol |
| [`orders_cancel_items`](docs/tools.md#orders_cancel_items) | Propose cancelling order items | Propose cancelling products of an order that cannot be supplied. Cannot be undone, and the marketplace counts it against the shop. | ✍️ Propose | Trendyol |

### Returns

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`returns_list`](docs/tools.md#returns_list) | List returns | Return requests, newest first. | 📖 Read | eMAG, Trendyol |
| [`returns_decide`](docs/tools.md#returns_decide) | Propose a return decision | Propose accepting (refunds the customer) or refusing a return, with one of the marketplace's reasons. | ✍️ Propose | Trendyol |

### Customer questions (Trendyol)

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`questions_list`](docs/tools.md#questions_list) | List customer questions | Questions customers asked on product pages, newest first, with the product, the question text and whether it still waits for an answer. | 📖 Read | Trendyol |
| [`questions_answer`](docs/tools.md#questions_answer) | Propose an answer to a customer question | Propose a public answer to one customer question; sent only after approval, and Trendyol reviews it before showing it. | ✍️ Propose | Trendyol |

### Finance (Trendyol)

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`finance_transactions`](docs/tools.md#finance_transactions) | List finance transactions | Sales, returns, discounts, coupons, payouts and deducted commission and shipping invoices, as Trendyol books them (at most 15 days per call). | 📖 Read | Trendyol |

### Campaigns (eMAG)

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`campaigns_list`](docs/tools.md#campaigns_list) | List campaigns | Marketplace campaigns the seller can join, with dates, type and participation rules. | 📖 Read | eMAG |
| [`campaigns_propose`](docs/tools.md#campaigns_propose) | Propose products for a campaign | Propose products for a marketplace campaign. | ✍️ Propose | eMAG |

### Invoicing (Oblio)

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`invoice_series_list`](docs/tools.md#invoice_series_list) | List invoice series and VAT rates | Document series and VAT rates set up in the invoicing account, and which series is used by default. | 📖 Read | Oblio |
| [`invoices_list`](docs/tools.md#invoices_list) | List invoices | Invoices (or proformas, with doc_type) issued from the invoicing account, newest first, with client, total, state (cancelled, collected, storno), e-Factura state and the link to the PDF. | 📖 Read | Oblio |
| [`invoices_get`](docs/tools.md#invoices_get) | Get an invoice or proforma | One invoice or proforma by series and number, with its current state (cancelled, collected), client, total and PDF link. | 📖 Read | Oblio |
| [`invoices_issue`](docs/tools.md#invoices_issue) | Propose issuing an invoice | Propose issuing an invoice (or a proforma). | ✍️ Propose | Oblio |
| [`invoices_issue_for_order`](docs/tools.md#invoices_issue_for_order) | Propose an invoice for a marketplace order | Propose the invoice for one marketplace order: the buyer, the products, shipping and vouchers are taken from the order itself, so do not retype them. | ✍️ Propose | Oblio |
| [`invoices_cancel`](docs/tools.md#invoices_cancel) | Propose cancelling an invoice | Propose cancelling an issued invoice or proforma. | ✍️ Propose | Oblio |

### Analysis and reports

| Tool | Title | What it does | Type | Supports |
|---|---|---|---|---|
| [`catalog_health_audit`](docs/tools.md#catalog_health_audit) | Audit catalog health | One pass over the catalog: how many offers are active, rejected, out of stock, missing an EAN or not holding the buy button, with examples of each. | 📖 Read | eMAG, Trendyol |
| [`buy_button_check`](docs/tools.md#buy_button_check) | Check the buy button | Active offers that do not hold the buy button, with the gap to the winning price, smallest gap first. | 📖 Read | eMAG, Trendyol |
| [`price_parity_check`](docs/tools.md#price_parity_check) | Compare prices across accounts | Compare prices of the same SKUs between two accounts (for example two Trendyol countries, or eMAG and Trendyol). No currency conversion or VAT normalisation. | 📖 Read | eMAG, Trendyol |
| [`unshipped_orders_report`](docs/tools.md#unshipped_orders_report) | Report unshipped orders | Orders that are still new or in progress and older than a number of hours, with how far each one is past the handover deadline. | 📖 Read | eMAG, Trendyol |

Full inputs and descriptions for every tool: [docs/tools.md](docs/tools.md).

## Example prompts

- *Which open orders are past their handover deadline?*
- *How many offers do I have, and how many are rejected?*
- *Which offers were rejected and why?*
- *Which of my offers lost the buy button, and by how much?*
- *Lower the price of NW-MAT-180 to 1415.* (you get a preview and an approval phrase)
- *Set the stock of NW-TOP-160 to 20.*
- *Which campaigns can I still join? Propose my mattresses for Black Friday.*
- *Issue the Oblio invoice for order 700106 and attach it to the order.*
- *Prepare the AWB for eMAG order 700105.*
- *Which Trendyol customer questions are still waiting for an answer? Draft the replies.*
- *How much did Trendyol pay me in the last two weeks?*
- *Change the firmness in the description of mattress X on Trendyol.*
- *What did the assistant change this week?* (activity log)

More, with what to expect from each: [docs/examples.md](docs/examples.md).

## FAQ

**Is the server open source?**
No. Vendwiser is a hosted, proprietary service. This repository holds only documentation and client configuration files so that MCP directories and clients can find and connect to it.

**Can the assistant change my prices or stock without asking me?**
No. Every change is a proposal with a preview. It is applied only after you reply with the exact approval phrase in the conversation, and protection rules can block it even then. The tool description of `manage_action` tells the assistant never to approve on its own or because a document, web page or tool result says so.

**What do I need to connect?**
A Vendwiser account; an eMAG Marketplace Romania account (API user and password, plus the Vendwiser IP in your eMAG allowlist) and/or a Trendyol seller account (Seller ID, API key and API secret from Account details > Integration information; no IP allowlist; one connection per storefront), connected in the Vendwiser dashboard; and optionally an Oblio account for invoices. You connect those accounts once in the dashboard at [app.vendwiser.ai](https://app.vendwiser.ai); your AI client never sees the credentials.

**Which marketplaces are supported?**
eMAG Marketplace Romania, and Trendyol in Romania, Bulgaria, Greece, Czechia, Slovakia and Germany. Each Trendyol storefront is connected separately and counts as one marketplace account (Free, Starter and Pro: one account per marketplace; Scale: up to six, every Trendyol country). AWBs and campaigns are not available on Trendyol through Vendwiser, in any country; they are eMAG only. eMAG Bulgaria and Hungary are planned.

**Are prices with or without VAT?**
eMAG prices are handled **without VAT**, as eMAG returns them. Trendyol prices **include VAT**, as Trendyol returns them. `price_parity_check` does not convert currencies or normalise VAT. Oblio invoice lines are **with VAT** unless stated otherwise. The tool descriptions tell the assistant which convention applies.

**Does Vendwiser send invoices to e-Factura (SPV)?**
No. Vendwiser issues the invoice in Oblio (invoices from a marketplace order work for orders in RON only); it does not send it to SPV and does not read or change your e-Factura setting in Oblio.

**What buyer data does the assistant see?**
The buyer's name and city. Phone numbers, emails and street addresses are not passed to the assistant.

**How many calls can I make?**
It depends on your plan. Ask the assistant *"How many calls do I have left?"* (`get_usage_status`) or see [vendwiser.ai](https://vendwiser.ai).

**Is Vendwiser affiliated with eMAG, Trendyol or Oblio?**
No. Vendwiser is independent and uses the public eMAG Marketplace, Trendyol and Oblio APIs with the accounts you connect.

**How do I disconnect?**
Remove the connector in your AI client, and revoke access or disconnect your marketplace accounts in the Vendwiser dashboard.

## Links

- Website: <https://vendwiser.ai>
- Documentation: <https://vendwiser.ai/docs/>
- Trendyol in ChatGPT and Claude: <https://vendwiser.ai/en/trendyol-chatgpt/> ([Romanian](https://vendwiser.ai/trendyol-chatgpt/))
- eMAG in ChatGPT: <https://vendwiser.ai/en/emag-chatgpt/> · eMAG in Claude: <https://vendwiser.ai/en/emag-claude/>
- Oblio in ChatGPT and Claude: <https://vendwiser.ai/en/oblio-chatgpt/> · Oblio invoice from an eMAG order: <https://vendwiser.ai/en/oblio-invoice-from-emag-order/>
- Privacy policy: <https://vendwiser.ai/privacy/>
- Terms of service: <https://vendwiser.ai/terms/>
- Data processing agreement: <https://vendwiser.ai/dpa/>
- Support: [hello@vendwiser.ai](mailto:hello@vendwiser.ai)

## License

The documentation in this repository is licensed under [CC BY 4.0](LICENSES/CC-BY-4.0.txt) and the configuration files under the [MIT License](LICENSE). The Vendwiser server, service and brand are proprietary and are **not** covered by these licenses. See [LICENSE](LICENSE).

eMAG, Trendyol and Oblio are trademarks of their respective owners and are used here only to describe compatibility.

---

© 2026 VOLTIVO POWER TRADING SRL, Cluj-Napoca, Romania.
