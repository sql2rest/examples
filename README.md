# SQL2REST Examples

Integration recipes and templates for **[SQL2REST](https://sql2rest.com)** — the read-only REST API for the JTL-Wawi.

These are safe-to-share building blocks for connecting SQL2REST to the tools you already use. They contain **no API keys and no customer data** — every example uses placeholders you replace with your own install's values.

## Available examples

| Example | What it does |
|---------|--------------|
| [`claude-desktop/`](./claude-desktop/) | Connect **Claude Desktop** to your live JTL-Wawi data via SQL2REST's HTTP-MCP endpoint, plus a ready-to-paste project template (DE + EN) that teaches Claude your data model |

More coming: n8n & Make workflows, Power BI connection guide, Postman/Insomnia collection, and the public OpenAPI spec.

## Requirements

A running **SQL2REST** install (v1.6+ for the MCP examples) on your JTL-Wawi server. Your MCP URL and API key are shown in the **API Dashboard → MCP**.

- Get SQL2REST: **[sql2rest.com](https://sql2rest.com)**
- Docs: **[sql2rest.com/docs](https://sql2rest.com/docs/)**

## Contributing

Found a useful integration? Open a pull request or an issue. Keep examples free of secrets and real data.
