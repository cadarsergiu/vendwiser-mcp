# Installing Vendwiser (instructions for AI agents such as Cline)

Vendwiser is a **hosted remote MCP server**. There is **nothing to clone, build, install or run locally**, and no API key or environment variable to set. Do not run `npm install`, `pip install` or any build step for this repository; it contains only documentation and configuration files.

## 1. Add the remote server

Add this entry to the MCP settings file of the client (for Cline: `cline_mcp_settings.json`, opened from **MCP Servers → Configure**). Keep any existing servers.

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

Alternatively, in Cline open **MCP Servers → Remote Servers** and add name `vendwiser` with URL `https://app.vendwiser.ai/mcp`.

## 2. Sign in (the user does this)

The server uses OAuth 2.1 with Dynamic Client Registration and PKCE. On first connection the client receives `401 Unauthorized` and must open the browser sign-in. In Cline, press **Authenticate** next to `vendwiser`. Ask the user to sign in to their Vendwiser account in the browser window and press **Allow access**. Never ask the user for their password, and never put tokens or passwords into configuration files.

Requirements on the user's side:

- A Vendwiser account (sign up at https://vendwiser.ai; a free plan is available).
- An eMAG Marketplace Romania seller account (API user and password, plus the Vendwiser IP in the eMAG allowlist) and/or a Trendyol seller account in Romania, Bulgaria, Greece, Czechia, Slovakia or Germany (Seller ID, API key and API secret; each storefront is connected separately, no IP allowlist), connected in the Vendwiser dashboard at https://app.vendwiser.ai. Oblio is optional (for invoices).

## 3. Verify

After sign-in, the server should list **52 tools**. Call `start_here` (read-only) to confirm the connection; it returns the connected accounts and remaining calls.

## Rules for using the tools

- Tools titled "Propose …" only simulate changes and return a preview and an action id. Show the preview and the approval phrase to the user.
- Call `manage_action` with `decision: "approve"` only after the user explicitly approves that exact change in the conversation.
- eMAG prices are without VAT; Trendyol prices include VAT.

## Troubleshooting

| Symptom | Fix |
|---|---|
| `401 Unauthorized` | Expected before sign-in. Start the OAuth flow (Authenticate). |
| Connected but no accounts | The user must connect their eMAG or Trendyol account in the Vendwiser dashboard. |
| Authentication or network error from eMAG or Trendyol | Call `connection_health`. |
| Client only supports stdio | Use `npx -y mcp-remote https://app.vendwiser.ai/mcp` as the command. |

Documentation: https://vendwiser.ai/docs/ · Support: hello@vendwiser.ai
