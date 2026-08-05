# SQL2REST + Claude Desktop — Project Template

Turn Claude into a JTL-Wawi analyst. This template gives Claude the domain knowledge
to use the SQL2REST API well; the MCP connector gives it the actual tool access.

- **MCP connector** → the *hands* (live read-only access to your JTL data)
- **This template** → the *brain* (knows your data model, conventions, and typical workflows)

You need both. The connector alone leaves Claude guessing at endpoints; the template
alone has knowledge but no tools.

> **Use Claude Desktop** (Windows, macOS or Linux). SQL2REST authenticates with a static
> `X-API-Key` header. Claude Desktop's **"Add connector" UI is OAuth-only** and has no
> API-key field, so the connection is made through a small **config-file entry** using the
> `mcp-remote` bridge (below). The **claude.ai web** connector is also OAuth-only and cannot
> connect a static-key install today (see "claude.ai web" below).
>
> **Requirements:** SQL2REST **v1.6+** with HTTP-MCP enabled (Phase 44.9), and **Node.js 20+**
> on the machine running Claude Desktop (the bridge needs it — see below).

---

## Deutsch

### Was du brauchst

1. Eine laufende **SQL2REST**-Installation (v1.6+) mit aktiviertem HTTP-MCP.
2. Die **MCP-URL** und den **API-Key** deiner Installation. Beides findest du im
   **API-Dashboard → MCP** (z. B. `http://dein-server:8000/mcp`).
3. **Claude Desktop** (Windows, macOS oder Linux). *Die claude.ai-Web-Version geht aktuell
   nicht — siehe Hinweis ganz oben.*
4. **Node.js 20 oder neuer** auf dem Rechner mit Claude Desktop. Prüfen mit `node -v`.
   Fehlt Node oder ist es älter als 20, von https://nodejs.org (LTS) installieren.

### Einrichtung in 3 Schritten

> **Wichtig:** Das Verbinden läuft in Claude Desktop über eine kleine **Konfigurations-Datei**,
> nicht über das „Connector hinzufügen"-Fenster. Dieses Fenster kann nur OAuth und hat
> **kein Feld für einen API-Key**. Wir tragen stattdessen einen kleinen Helfer namens
> `mcp-remote` in die Datei ein, der deinen Key mitschickt. Klingt technisch, ist aber
> Copy-Paste. Einmal eingerichtet, fertig für immer.

**Schritt 1 — Konfig-Datei öffnen**

In Claude Desktop: **Einstellungen → Entwickler → „Konfiguration bearbeiten"**
(*Settings → Developer → Edit Config*). Das öffnet die Datei `claude_desktop_config.json`
im Editor. Manuell liegt sie unter:
- **Windows:** `%APPDATA%\Claude\`
- **macOS:** `~/Library/Application Support/Claude/`
- **Linux:** `~/.config/Claude/`

Ist die Datei leer, schreib einfach `{}` hinein.

**Schritt 2 — diesen Block einfügen**

Trage den `mcpServers`-Block ein und ersetze **URL** und **API-Key** durch die Werte aus
dem **API-Dashboard → MCP**:

```json
{
  "mcpServers": {
    "SQL2REST": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "http://DEIN-SERVER:8000/mcp",
        "--transport", "http-only",
        "--header", "X-API-Key:${SQL2REST_KEY}",
        "--no-auth"
      ],
      "env": { "SQL2REST_KEY": "DEIN-API-KEY" }
    }
  }
}
```

> - Der Key steht bewusst unten bei `env` und wird oben als `${SQL2REST_KEY}` referenziert —
>   so vermeiden wir einen Windows-Quoting-Fehler. **Kein Leerzeichen** nach `X-API-Key:`.
> - Läuft dein SQL2REST ohne HTTPS (z. B. im lokalen Netz), bleibt `http://` korrekt. Mit
>   HTTPS-Reverse-Proxy stattdessen `https://...`.
> - Hattest du vorher schon andere Server in der Datei, füge nur die
>   `"SQL2REST": { ... }`-Zeile innerhalb von `mcpServers` hinzu (Komma nicht vergessen).
> - **Als UTF-8 OHNE BOM speichern.** Sonst meldet Claude Desktop beim Start
>   *„Could not load app settings / Unexpected token … is not valid JSON"*.

Speichern und **Claude Desktop komplett neu starten** — nicht nur das Fenster schließen,
sondern über das **System-Tray → Rechtsklick → Beenden** ganz schließen, dann neu öffnen.
Beim ersten Start lädt `npx` den Helfer kurz im Hintergrund. Unter **Einstellungen →
Entwickler** sollte „SQL2REST" nach ein paar Sekunden als verbunden erscheinen.

**Schritt 3 — Projekt anlegen und Anweisungen einfügen**

Neues Projekt erstellen, z. B. „JTL-Wawi (SQL2REST)". Dann den **kompletten** Inhalt von
[`system-instructions.de.md`](./system-instructions.de.md) (oder die englische Variante)
in die **Projekt-Anweisungen / Custom Instructions** kopieren. Fertig.

### Testen

Frag im Projekt z. B.:
- „Was waren die letzten 3 Bestellungen?"
- „Zeig mir die Top-10-Kunden nach Umsatz im Mai 2026."
- „Lagerbestand für SKU ABC-123?"

Claude sollte die SQL2REST-Tools aufrufen und mit echten Zahlen antworten.

### Wenn etwas klemmt

- **„Could not load app settings / Unexpected token … is not valid JSON":** Die Datei wurde
  mit einem BOM gespeichert → als **UTF-8 ohne BOM** neu speichern (z.B. in VS Code unten rechts
  auf die Kodierung klicken → „Save with Encoding" → „UTF-8").
- **„SQL2REST: Server disconnected" / Status `failed`:** Fast immer **Node zu alt**. `mcp-remote`
  braucht **Node 20+**; unter Node 18 stürzt es mit `undici: File is not defined` ab. `node -v`
  prüfen und auf 20+ aktualisieren. **Wichtig bei nvm/Linux:** Claude Desktop erbt die
  nvm-Umgebung der Shell NICHT — stelle sicher, dass die Standard-Node-Version 20+ ist und
  starte Desktop danach neu, oder trage den vollen Pfad zur npx aus deiner Node-20-Installation
  in `command` ein und setze `env.PATH` entsprechend.
- **Änderung wirkt nicht:** Nur das Fenster zu schließen reicht nicht — die App läuft im Tray
  weiter. Immer **komplett beenden** und neu starten.

### Mehrere Mitarbeiter (Agentur)

Jeder Mitarbeiter trägt den Block einmal in **seine eigene** `claude_desktop_config.json`
ein und legt ein Projekt mit demselben Template an. Der API-Key liegt nur lokal in der
Konfig-Datei, **nicht im Template** — das Template kann also unbedenklich geteilt werden.

---

## English

### What you need

1. A running **SQL2REST** install (v1.6+) with HTTP-MCP enabled.
2. Your install's **MCP URL** and **API key** (shown in **API Dashboard → MCP**,
   e.g. `http://your-server:8000/mcp`).
3. **Claude Desktop** (Windows, macOS or Linux). *The claude.ai web version does not work
   today — see the note at the top.*
4. **Node.js 20 or newer** on the machine running Claude Desktop. Check with `node -v`;
   install the LTS from https://nodejs.org if missing or older than 20.

### Setup in 3 steps

> **Important:** in Claude Desktop you connect via a small **config file**, not the
> "Add connector" dialog (that dialog is OAuth-only, no API-key field). We put a small
> bridge called `mcp-remote` in the file, which sends your key. Copy-paste; set up once.

**Step 1 — open the config file**

**Settings → Developer → "Edit Config"** opens `claude_desktop_config.json`. Manually:
- **Windows:** `%APPDATA%\Claude\`
- **macOS:** `~/Library/Application Support/Claude/`
- **Linux:** `~/.config/Claude/`

If the file is empty, put `{}` in it.

**Step 2 — paste this block**

Add the `mcpServers` block and replace **URL** and **API key** with the values from
**API Dashboard → MCP**:

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

> - The key stays in `env` (referenced as `${SQL2REST_KEY}`) to avoid Windows quoting issues.
>   **No space** after `X-API-Key:`.
> - No HTTPS (local network)? `http://` is correct. With an HTTPS reverse proxy use `https://`.
> - Already had other servers? Just add the `"SQL2REST": { ... }` entry inside `mcpServers`.
> - **Save as UTF-8 WITHOUT a BOM**, or Claude Desktop shows *"Could not load app settings /
>   Unexpected token … is not valid JSON"* on launch.

Save and **fully quit Claude Desktop** — not just the window; **system tray → right-click →
Quit** — then reopen. On first run `npx` fetches the bridge briefly. "SQL2REST" should appear
as connected under **Settings → Developer** after a few seconds.

**Step 3 — create a project and paste the instructions**

Create a project, e.g. "JTL-Wawi (SQL2REST)", and copy the **entire**
[`system-instructions.en.md`](./system-instructions.en.md) into the project's **Custom
Instructions**. Done.

### Test it

- "What were the last 3 orders?"
- "Show me the top 10 customers by revenue in May 2026."
- "Stock for SKU ABC-123?"

### Troubleshooting

- **"Could not load app settings / Unexpected token … is not valid JSON":** the file was saved
  with a BOM → re-save as **UTF-8 without BOM** (i.e. VS Code: click the encoding at bottom-right →
  "Save with Encoding" → "UTF-8").
- **"SQL2REST: Server disconnected" / status `failed`:** almost always **Node too old**.
  `mcp-remote` needs **Node 20+**; on Node 18 it crashes with `undici: File is not defined`.
  Check `node -v`. **With nvm/Linux:** Claude Desktop does NOT inherit your shell's nvm Node —
  make sure the default Node is 20+ and relaunch Desktop, or pin the absolute nvm npx path in
  `command` and set `env.PATH` to that bin directory first.
- **Change didn't apply:** closing the window isn't enough (app stays in the tray). **Fully
  quit** and restart.

### Multiple team members (agency)

Each person adds the block once to **their own** `claude_desktop_config.json` and creates a
project with the same template. The API key lives only locally in the config file, **never in
the template** — so the template is safe to share.

---

## Why not the claude.ai web app?

The claude.ai **web** custom-connector UI is **OAuth-only** — no field for an API key or custom
header (Anthropic closed the feature request as "not planned"). Since SQL2REST authenticates
with a static `X-API-Key`, a stock install can't be connected from the web app today. Use
**Claude Desktop** (above). *(Future option: an OAuth layer or reverse proxy in front of
SQL2REST would unlock the web app — not built yet.)*

> **Note on the native `type: "http"` config block:** some older Claude Desktop builds accepted
> a direct `{ "type": "http", "url": ..., "headers": { "X-API-Key": ... } }` entry. Current
> builds (verified 1.24012.9, Aug 2026) **silently strip it** and never connect — use the
> `mcp-remote` block above.

---

## Files

| File | Purpose |
|------|---------|
| [`system-instructions.de.md`](./system-instructions.de.md) | German custom instructions — paste into the project |
| [`system-instructions.en.md`](./system-instructions.en.md) | English custom instructions |
| `README.md` | This setup guide |

## Notes

- The template is intentionally **provider-agnostic about your data** — no customer names,
  SKUs, or numbers are hardcoded. It describes *how* SQL2REST works, not *what* is in your DB.
- Keep the template in sync with the API: the tool list mirrors SQL2REST's MCP tools. If a
  future release adds tools, update the "Available tools" section.
- Full API reference: Swagger UI at `/docs` on your SQL2REST server.
