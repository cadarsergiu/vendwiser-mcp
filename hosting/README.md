# Files to host on your own domains

These files are **not** used from GitHub. They are drafts to copy to the Vendwiser web servers. See [PUBLISHING.md](../PUBLISHING.md) (Romanian) for the full steps.

| File in this folder | Where to serve it | Used by |
|---|---|---|
| `.well-known/mcp/server-card.json` | `https://app.vendwiser.ai/.well-known/mcp/server-card.json` (`Content-Type: application/json`, public, no auth) | Smithery and other scanners that cannot pass the OAuth sign-in. Format: Smithery static server card (SEP-1649 draft). Regenerate it whenever the tool list changes. |
| `mcp-registry-auth` (**not included**, you generate it) | `https://vendwiser.ai/.well-known/mcp-registry-auth` (`text/plain`, a single line) | Official MCP Registry, HTTP domain verification. Contains only `v=MCPv1; k=ed25519; p=<public key>`. Never publish the private key. |

The server card lists the same 79 tools (names, titles, descriptions, input schemas and annotations) that `tools/list` returns. `serverInfo.version` is set to `1.2.0`; change it if the server reports a different version in `initialize`.
