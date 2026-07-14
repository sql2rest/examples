# SQL2REST + Claude Desktop — Project Template

Turn Claude into a JTL-Wawi analyst. This template gives Claude the domain knowledge
to use the SQL2REST API well; the MCP connector gives it the actual tool access.

- **MCP connector** → the *hands* (live read-only access to your JTL data)
- **This template** → the *brain* (knows your data model, conventions, and typical workflows)

You need both. The connector alone leaves Claude guessing at endpoints; the template
alone has knowledge but no tools.

> **Use Claude Desktop.** SQL2REST authenticates with a static `X-API-Key` header,
> which Claude Desktop supports via its config file. The **claude.ai web** connector
> is currently **OAuth-only** and has no API-key/header field, so a static-key
> SQL2REST install can't be connected there today (see "claude.ai web" below).
> Requires SQL2REST **v1.6+** with HTTP-MCP enabled (Phase 44.9).

---

## Deutsch

### Was du brauchst

1. Eine laufende **SQL2REST**-Installation (v1.6+) mit aktiviertem HTTP-MCP.
2. Die **MCP-URL** und den **API-Key** deiner Installation. Beides findest du im
   **API-Dashboard → MCP** (z. B. `http://dein-server:8000/mcp`).
3. **Claude Desktop** (Windows oder Mac). *Die claude.ai-Web-Version geht aktuell nicht —
   siehe Hinweis ganz oben.*

### Einrichtung in 3 Schritten

> **Wichtig:** Das Verbinden läuft in Claude Desktop über eine kleine **Konfigurations-Datei**,
> nicht über das „Connector hinzufügen"-Fenster. Dieses Fenster kann nur OAuth und hat
> **kein Feld für einen API-Key** — der SQL2REST-Key muss in die Datei. Klingt technisch,
> ist aber Copy-Paste. Einmal eingerichtet, fertig für immer.

**Schritt 1 — Konfig-Datei öffnen**

In Claude Desktop: **Einstellungen → Entwickler → „Konfiguration bearbeiten"**
(*Settings → Developer → Edit Config*). Das öffnet die Datei `claude_desktop_config.json`
im Editor. (Manuell liegt sie unter Windows in `%APPDATA%\Claude\`, auf dem Mac unter
`~/Library/Application Support/Claude/`.) Ist die Datei leer, schreib einfach `{}` hinein.

**Schritt 2 — diesen Block einfügen**

Trage den `mcpServers`-Block ein und ersetze **URL** und **API-Key** durch die Werte aus
dem **API-Dashboard → MCP**:

```json
{
  "mcpServers": {
    "SQL2REST": {
      "type": "http",
      "url": "http://DEIN-SERVER:8000/mcp",
      "headers": {
        "X-API-Key": "DEIN-API-KEY"
      }
    }
  }
}
```

> Läuft dein SQL2REST ohne HTTPS (z. B. im lokalen Netz), bleibt `http://` korrekt —
> genau so wie oben. Mit HTTPS-Reverse-Proxy stattdessen `https://...`. Hattest du
> vorher schon andere Server in der Datei, füge nur die `"SQL2REST": { ... }`-Zeile
> innerhalb von `mcpServers` hinzu (Komma zwischen den Einträgen nicht vergessen).

Speichern und **Claude Desktop komplett neu starten** (beenden und neu öffnen). Unter
🔌 bzw. in den Tools sollte „SQL2REST" jetzt auftauchen.

**Schritt 3 — Projekt anlegen und Anweisungen einfügen**

Neues Projekt erstellen, z. B. „JTL-Wawi (SQL2REST)". Dann den **kompletten** Inhalt von
[`system-instructions.de.md`](./system-instructions.de.md) (oder die englische Variante)
in die **Projekt-Anweisungen / Custom Instructions** kopieren. Fertig.

> **Geht der `type: "http"`-Block bei einer älteren Claude-Desktop-Version nicht?**
> Dann den Fallback weiter unten (`mcp-remote`) verwenden.

### Testen

Frag im Projekt z. B.:
- „Zeig mir die Top-10-Kunden nach Umsatz im Mai 2026."
- „Welche Artikel gibt es in Farbe Rot, Größe M?"
- „Lagerbestand für SKU ABC-123?"

Claude sollte die SQL2REST-Tools aufrufen und mit echten Zahlen antworten.

### Mehrere Mitarbeiter (Agentur)

Jeder Mitarbeiter trägt den Block einmal in **seine eigene** `claude_desktop_config.json`
ein und legt ein Projekt mit demselben Template an. Der API-Key liegt nur lokal in der
Konfig-Datei, **nicht im Template** — das Template kann also unbedenklich geteilt werden.
In Multi-Mandanten-Setups klärt Claude von sich aus, welcher Mandant gemeint ist
(siehe Template).

---

## English

### What you need

1. A running **SQL2REST** install (v1.6+) with HTTP-MCP enabled.
2. Your install's **MCP URL** and **API key**. Both are shown in the
   **API Dashboard → MCP** (e.g. `http://your-server:8000/mcp`).
3. **Claude Desktop** (Windows or Mac). *The claude.ai web version does not work today —
   see the note at the top.*

### Setup in 3 steps

> **Important:** in Claude Desktop you connect via a small **config file**, not the
> "Add connector" dialog. That dialog is OAuth-only and has **no API-key field** — the
> SQL2REST key goes in the file. Sounds technical, but it's copy-paste. Set up once,
> done forever.

**Step 1 — open the config file**

In Claude Desktop: **Settings → Developer → "Edit Config"**. This opens
`claude_desktop_config.json` in your editor. (Manually: Windows `%APPDATA%\Claude\`,
Mac `~/Library/Application Support/Claude/`.) If the file is empty, put `{}` in it.

**Step 2 — paste this block**

Add the `mcpServers` block and replace **URL** and **API key** with the values from
the **API Dashboard → MCP**:

```json
{
  "mcpServers": {
    "SQL2REST": {
      "type": "http",
      "url": "http://YOUR-SERVER:8000/mcp",
      "headers": {
        "X-API-Key": "YOUR-API-KEY"
      }
    }
  }
}
```

> If your SQL2REST runs without HTTPS (e.g. on a local network), `http://` is correct —
> exactly as above. With an HTTPS reverse proxy, use `https://...` instead. If you
> already had other servers in the file, just add the `"SQL2REST": { ... }` entry inside
> `mcpServers` (don't forget the comma between entries).

Save and **fully restart Claude Desktop** (quit and reopen). "SQL2REST" should now show
up under 🔌 / in the tools list.

**Step 3 — create a project and paste the instructions**

Create a new project, e.g. "JTL-Wawi (SQL2REST)". Then copy the **entire** contents of
[`system-instructions.en.md`](./system-instructions.en.md) (or the German variant) into
the project's **Instructions / Custom Instructions**. Done.

> **Does the `type: "http"` block not work on an older Claude Desktop version?**
> Use the `mcp-remote` fallback below.

### Test it

In the project, ask e.g.:
- "Show me the top 10 customers by revenue in May 2026."
- "Which products come in red, size M?"
- "Stock for SKU ABC-123?"

Claude should call the SQL2REST tools and answer with real figures.

### Multiple team members (agency)

Each person adds the block once to **their own** `claude_desktop_config.json` and
creates a project with the same template. The API key lives only locally in the config
file, **never in the template** — so the template is safe to share. In multi-tenant
(Mandanten) setups, Claude clarifies which tenant is meant on its own (see the template).

---

## Fallback: `mcp-remote` (older Claude Desktop)

If your Claude Desktop is too old to support the native `type: "http"` block, wrap the
remote server in the `mcp-remote` bridge (needs Node.js / `npx` on the machine):

```json
{
  "mcpServers": {
    "SQL2REST": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "http://YOUR-SERVER:8000/mcp",
        "--transport", "http-only",
        "--header", "X-API-Key:${SQL2REST_KEY}",
        "--no-auth"
      ],
      "env": { "SQL2REST_KEY": "YOUR-API-KEY" }
    }
  }
}
```

The key is kept in `env` (not in `args`) to avoid Windows quoting issues. `--no-auth`
skips mcp-remote's automatic OAuth attempt; `--header` injects the static API key.

## Why not the claude.ai web app?

The claude.ai **web** custom-connector UI is **OAuth-only** — it has no field for an
API key or custom header (Anthropic closed the feature request as "not planned"). Since
SQL2REST authenticates with a static `X-API-Key`, a stock install can't be connected
from the web app today. Use **Claude Desktop** (above). *(Future option: an OAuth layer
or a reverse proxy in front of SQL2REST would unlock the web app — not built yet.)*

---

## Files

| File | Purpose |
|------|---------|
| [`system-instructions.de.md`](./system-instructions.de.md) | German custom instructions — paste into the project |
| [`system-instructions.en.md`](./system-instructions.en.md) | English custom instructions |
| `README.md` | This setup guide |

## Notes

- The template is intentionally **provider-agnostic about your data** — no customer
  names, SKUs, or numbers are hardcoded. It describes *how* SQL2REST works, not *what*
  is in your database.
- Keep the template in sync with the API: the tool list mirrors SQL2REST's 23 MCP
  tools (17 sales + 6 procurement). If a future release adds tools, update the
  "Available tools" section.
- Full API reference: Swagger UI at `/docs` on your SQL2REST server.
