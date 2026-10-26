# SQL2REST Examples

Integration recipes and templates for **[SQL2REST](https://sql2rest.com)** — the read-only REST API for the JTL-Wawi.

These are safe-to-share building blocks for connecting SQL2REST to the tools you already use. They contain **no API keys and no customer data** — every example uses placeholders you replace with your own install's values.

## Available examples

| Example | What it does |
|---------|--------------|
| [`ai-project-template/`](./ai-project-template/) | System instructions (DE + EN) that teach **Claude** or **ChatGPT** your JTL-Wawi data model: which tool answers which question, and the traps that produce wrong numbers. Paste into a project. Includes both connection routes, sign-in connector and configuration file |
| [`n8n-templates/`](./n8n-templates/) | Importable **n8n** workflows: a generic "call any endpoint" starter plus flagship recipes (new-orders notification, daily product export, low-stock alert). Header Auth against your single API key |
| [`make-templates/`](./make-templates/) | Importable **Make** blueprints: a generic HTTP starter plus a new-orders notification, using the generic HTTP module with an `X-API-Key` header |
| [`zapier-templates/`](./zapier-templates/) | Step-by-step guide to call any SQL2REST endpoint from **Zapier** via "Webhooks by Zapier" (no custom Zapier app needed) |

More coming: Power BI connection guide, Postman/Insomnia collection, and the public OpenAPI spec.

## Requirements

A running **SQL2REST** install (v1.6+ for the MCP examples) on your JTL-Wawi server. Your MCP URL and API key are shown in the **API Dashboard → MCP**.

- Get SQL2REST: **[sql2rest.com](https://sql2rest.com)**
- Docs: **[sql2rest.com/docs](https://sql2rest.com/docs/)**

## Contributing

Found a useful integration? Open a pull request or an issue. Keep examples free of secrets and real data.

<!-- Templates synced to SQL2REST v1.2.10 — see https://sql2rest.com/changelog/ -->
