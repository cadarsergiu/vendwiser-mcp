# Changelog

All notable changes to this documentation and configuration repository are listed here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versions follow [Semantic Versioning](https://semver.org/). The version matches `server.json`, `gemini-extension.json`, `.cursor-plugin/plugin.json` and `lhm.plugin.json`; bump all four together and create a matching Git tag (for example `v1.0.0`).

## [Unreleased]

## [1.2.0] - 2026-10-10

### Added

- **Kaufland Global Marketplace** in Germany, Austria, Poland, Czechia, Slovakia, France, Italy, Spain and the Netherlands. Each country is a separate marketplace account, connected with the Client key and Secret key from the Kaufland Seller Portal (same keys for every country, no IP allowlist). Kaufland prices include VAT, in the currency of each country.
- 27 tools (79 in total): `offers_competitors`, `market_insights`, `shipping_groups_list`, `offers_update_shipping_group`, `offers_update_handling_time`, `offers_unlock`, `seller_addresses`, `orders_label_get`, `order_notifications_get`, `order_notifications_set`, `orders_mark_shipped`, `orders_mark_delivered`, `orders_mark_invoiced`, `orders_set_parcels`, `orders_change_courier`, `orders_detach_invoice`, `orders_refund`, `orders_contact_buyer`, `returns_history`, `returns_create`, `questions_close`, `reports_list`, `reports_request`, `campaigns_check`, `vouchers_list`, `vouchers_create`, `vouchers_stop`.
- Links to the Kaufland pages: <https://vendwiser.ai/en/kaufland/> and <https://vendwiser.ai/kaufland/>.
- Kaufland examples in `docs/examples.md`.

### Changed

- Scale plan: up to nine accounts per marketplace (every Trendyol or Kaufland country), was six.
- Existing tools now list Kaufland where the server supports it (offers, prices, stock, status, product pages, new products, orders, returns, questions/tickets). Product page content (`offers_content_get`, `offers_update_content`) now also lists eMAG Romania, as the live `tools/list` does.
- Live schema changes: `returns_decide` accepts `repair` and `clarify` where the marketplace has them; `questions_answer` has `interim` (tickets); `offers_update_price` has `min_price`; `offers_create` has `storefront_prices` (Kaufland, one offer per country); `unshipped_orders_report` accepts `hours`.
- "Works on" explained as per-workspace: the live `tools/list` lists the accounts the user has connected. `docs/tools.md`, the server card and `lhm.plugin.json` now say **Supports:** with the full list of marketplaces.
- AWB and campaigns: "not available on Trendyol through Vendwiser, in any country".
- README (EN/RO): links to the English and Romanian content pages on vendwiser.ai.
- Links moved to the new site structure: integrations hub (`/integrari/`, `/en/integrations/`), showcase pages with example conversations and videos (`/emag/`, `/trendyol/`, `/oblio/` and `/en/…`), setup guides (`/emag-chatgpt/`, `/emag-claude/`) and `llms-full.txt`. The old `/trendyol-chatgpt/` and `/oblio-chatgpt/` pages now redirect.
- `server.json` description now mentions Kaufland; version 1.2.0 in `server.json`, `gemini-extension.json`, `.cursor-plugin/plugin.json`, `lhm.plugin.json` and the server card.

### Notes

- Kaufland status: reading and stock changes are verified on a real Kaufland account. Price changes, ticket replies, vouchers, marking orders shipped and the other Kaufland changes are built and go through the same preview and approval, but are still being verified on real accounts; they are marked "changes being verified on real accounts" in `docs/tools.md` and † in the README tool tables.
- Titles and annotations of the 27 new tools in `docs/tools.md` and the server card are documentation labels until the next server card export.

## [1.1.0] - 2026-10-10

### Added

- **Trendyol** support documented: Trendyol Romania, Bulgaria, Greece, Czechia, Slovakia and Germany, each storefront connected separately with Seller ID, API key and API secret (no IP allowlist).
- 10 Trendyol-only tools in `docs/tools.md`, the server card and `lhm.plugin.json`: `offers_content_get`, `offers_update_content`, `offers_create`, `offers_delete`, `orders_split`, `orders_cancel_items`, `returns_decide`, `questions_list`, `questions_answer`, `finance_transactions` (52 tools in total).
- "Where each tool works" section in `docs/tools.md`; every marketplace tool description now ends with "Works on: …".

### Changed

- README (EN and RO), `server.json`, `gemini-extension.json`, `.cursor-plugin/plugin.json`, `lhm.plugin.json`, `GEMINI.md`, `rules/vendwiser.mdc`, `llms-install.md` and `docs/security.md` describe eMAG Romania, Trendyol and Oblio.
- VAT conventions documented: eMAG prices exclude VAT, Trendyol prices include VAT; `price_parity_check` does not convert currencies or normalise VAT.
- Clarified that AWBs and campaigns are eMAG only, and that invoices from a marketplace order work for RON orders only (eMAG Romania, Trendyol Romania).
- Version bumped to 1.1.0 in all manifests.

## [1.0.0] - 2026-10-08

### Added

- README in English and Romanian with connection instructions for Claude, Claude Code, ChatGPT, Cursor, VS Code, Gemini CLI, Cline, Windsurf and generic MCP clients.
- Reference for all 42 tools (`docs/tools.md`), security and data handling (`docs/security.md`) and example prompts (`docs/examples.md`).
- `server.json` for the official MCP Registry (`ai.vendwiser/vendwiser`, Streamable HTTP remote).
- `gemini-extension.json` and `GEMINI.md` for Gemini CLI.
- `.cursor-plugin/plugin.json`, `mcp.json` and `rules/vendwiser.mdc` for Cursor.
- `lhm.plugin.json` for the LobeHub MCP Marketplace.
- `llms-install.md` for Cline and other agents.
- Static server card draft (`hosting/.well-known/mcp/server-card.json`).
- Logo (400, 500, 512 and 1024 px) and 1200×200 banner.
