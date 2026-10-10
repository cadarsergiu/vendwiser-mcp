# Changelog

All notable changes to this documentation and configuration repository are listed here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versions follow [Semantic Versioning](https://semver.org/). The version matches `server.json`, `gemini-extension.json`, `.cursor-plugin/plugin.json` and `lhm.plugin.json`; bump all four together and create a matching Git tag (for example `v1.0.0`).

## [Unreleased]

### Changed

- Scale plan: up to six accounts per marketplace (every Trendyol country), was five.
- "Works on" explained as per-workspace: the live `tools/list` lists the accounts the user has connected. `docs/tools.md`, the server card and `lhm.plugin.json` now say **Supports:** with the full list of marketplaces.
- AWB and campaigns: "not available on Trendyol through Vendwiser, in any country".
- README (EN/RO): links to the English and Romanian content pages on vendwiser.ai.

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
