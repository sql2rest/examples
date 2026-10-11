# SQL2REST — AI Project Template (Claude and ChatGPT)

Turn your AI assistant into a JTL-Wawi analyst. This template gives it the domain
knowledge to use the SQL2REST API well; the MCP connection gives it the actual tool
access.

**The system instruction works in both Claude and ChatGPT** — paste it into a project
in either one. The setup walkthrough below is written for Claude, because that is the
route with three options (sign-in connector, configuration file on the server itself,
configuration file on another computer). For ChatGPT you only need the public MCP address
(Team/Agency plan) and the instruction; that path is documented at
https://sql2rest.com/jtl-wawi/chatgpt/

- **MCP connection** → the *hands* (live read-only access to your JTL data)
- **This template** → the *brain* (knows your data model, conventions, and typical workflows)

You need both. The connection alone leaves Claude guessing at endpoints; the template
alone has knowledge but no tools.

> **Three routes, and your plan decides which ones are yours.** The question is where the
> AI client runs: on the server with SQL2REST itself, or on another computer.
>
> | | A: Connector (OAuth sign-in) | B: Configuration file on the server itself | C: Configuration file on another computer |
> |---|---|---|---|
> | Plan | **Team / Agency** | **every plan** (Personal and trial included) | **Team / Agency** |
> | Works in | claude.ai in the browser, Claude Desktop, the phone and tablet apps | Claude Desktop, Claude Code, Cursor, Windsurf, running on the SQL2REST server | Claude Desktop, Claude Code, Cursor, Windsurf, on another computer in your network or anywhere on the internet |
> | Needs | SQL2REST **v1.4.0+** and the built-in HTTPS tunnel | nothing extra: the client starts `sql2rest.exe --mcp` itself | the switch "Allow MCP from other computers", an API key and **Node.js 20+** on the client machine |
> | Setup | sign in once, approve | paste a JSON block, restart | paste a JSON block, restart |
>
> **Below Team (Personal, trial) only route B exists.** MCP from other computers, whether
> from your own network or from the internet, is part of the Team plan. On the server
> itself, MCP works on every plan.
>
> Since **v1.4.0** the connector is the normal route for Claude on Team and Agency, and
> nobody hand-edits `claude_desktop_config.json` for it any more. The configuration-file
> routes stay fully supported: B is the route on the Personal plan, C the route for
> clients that cannot sign in through a connector yet (Cursor, Windsurf).
>
> Step by step with screenshots: **https://sql2rest.com/jtl-wawi/claude/**

---

## Deutsch

### Weg A — Konnektor (Team-/Agency-Tarif, empfohlen)

SQL2REST als eigenen Konnektor in Claude hinzufügen, anmelden, fertig. Keine
Konfigurationsdatei, kein zusätzlicher Prozess auf dem Rechner, kein API-Schlüssel,
der irgendwo eingetragen werden muss. Es läuft im Browser auf claude.ai, in Claude
Desktop und in den Claude-Apps am Handy und Tablet.

**Was du dafür brauchst**

1. **SQL2REST v1.4.0 oder neuer**, auf dem Server, wo auch deine JTL-Wawi liegt.
2. **Ein HTTPS-Tunnel.** Seit v1.3.29 eingebaut, du musst nichts zusätzlich installieren.
3. **Team- oder Agency-Tarif.** Der Konnektor braucht eine öffentliche Adresse, und die
   gibt es ab dem Team-Tarif. Im Personal-Tarif und in der Testphase nimmst du **Weg B**.

**In fünf Schritten verbunden**

1. In Claude auf **Einstellungen → „Anpassen" → „Konnektoren"**. Oben rechts über
   **„Hinzufügen"** den Punkt **„Benutzerdefinierten Connector hinzufügen"** wählen.
2. **Die Adresse deines Systems eintragen:** deine öffentliche Adresse mit `/mcp` am
   Ende, ohne Schrägstrich dahinter — z. B. `https://dein-tunnel.jtl2api.com/mcp`.
   Die Adresse steht in deinem **Mitglieder-Portal auf sql2rest.com unter „Deine
   Adressen"**, in der Zeile MCP, und im API-Dashboard unter **Einstellungen →
   Remote-Zugriff**. Einmal auf Kopieren, fertig.
3. **Verbindung starten.** Du landest auf der SQL2REST-Anmeldung und bekommst einen
   Magic Link per E-Mail. Ein Passwort brauchst du nicht.
4. **Freigeben.** Vor dem Bestätigen siehst du, um welches Konto es geht, welches System
   angebunden wird und welche Datenbereiche freigegeben werden.
5. **Fertig.** Der Konnektor steht in deiner Liste. Er gehört zu deinem Claude-Konto,
   gilt also auch im Browser und am Handy. In Claude Desktop erscheint er nach einem
   Neustart.

**Was Claude lesen darf, bestimmst du.** Die Anmeldung läuft über OAuth, also so, wie du
es von „Mit Google anmelden" kennst: Claude bekommt keinen Schlüssel, sondern eine eigene
Freigabe pro Person, die du jederzeit einzeln zurückziehen kannst. Freigegeben wird immer
nur die Schnittmenge aus dem, was dein Administrator dir zugewiesen hat, und dem, was die
App anfragt. Im Reiter **„Schlüssel"** stehen Personen, ihre verbundenen Apps und ihre
Schlüssel an einer Stelle; jede Verbindung lässt sich einzeln trennen, ohne der Person
den Zugang zu nehmen.

Weiter unten bei **„Projekt anlegen und Systemanweisung einfügen"**.

### Weg B — Konfigurationsdatei am Server selbst (jeder Tarif)

Claude Desktop, Claude Code, Cursor oder Windsurf laufen direkt auf dem Server, auf dem
auch SQL2REST läuft. Das Programm startet SQL2REST dann selbst, ohne Adresse und ohne
Netzwerk. Das geht in jedem Tarif und ist im Personal-Tarif und in der Testphase der
einzige Weg: MCP von anderen Rechnern, ob aus dem eigenen Netzwerk oder aus dem Internet,
gehört zum Team-Tarif.

> **Ein Punkt, der viele überrascht:** Die Konfigurationsdatei startet den MCP-Server in
> einer lokalen Sitzung auf dem Rechner, auf dem Claude läuft. Auf gehosteten
> Windows-Servern, und genau dort liegt die JTL-Wawi meistens, lässt sich diese Sitzung
> oft gar nicht starten. Dann bleibt ab dem Team-Tarif Weg A oder Weg C.

**Was du dafür brauchst**

1. Eine laufende **SQL2REST**-Installation (Windows-Dienst) auf diesem Server.
2. Den Pfad zu `sql2rest.exe`, meist `C:\sql2rest\sql2rest.exe`. Einen Schalter im
   API-Dashboard, einen API-Key oder Node.js brauchst du nicht: SQL2REST liest den Schlüssel
   selbst aus seiner Konfiguration.

**Schritt 1 — Konfig-Datei öffnen** wie unten bei Weg C beschrieben (in Claude Desktop:
**Einstellungen → Entwickler → „Konfiguration bearbeiten"**).

**Schritt 2 — diesen Block einfügen**

```json
{
  "mcpServers": {
    "sql2rest": {
      "command": "C:\\sql2rest\\sql2rest.exe",
      "args": ["--mcp"]
    }
  }
}
```

> - Liegt SQL2REST woanders, den Pfad anpassen. Im JSON stehen die Backslashes doppelt.
> - Wie bei Weg C gilt: der `mcpServers`-Block gehört **innerhalb** des vorhandenen
>   `{ }`-Root-Objekts, mit Komma davor, und die Datei wird als **UTF-8 ohne BOM** gespeichert.
> - Cursor und Windsurf nehmen denselben Block in ihrer eigenen MCP-Konfiguration.

Speichern und **Claude Desktop komplett neu starten** (System-Tray → Rechtsklick → Beenden).
Danach erscheint „sql2rest" unter **Einstellungen → Entwickler** als verbunden.

### Weg C — Konfigurationsdatei von einem anderen Rechner (Team-/Agency-Tarif)

Claude Desktop, Claude Code, Cursor oder Windsurf laufen auf einem anderen Rechner als
SQL2REST, im eigenen Netzwerk oder irgendwo im Internet. Dieser Weg ist **MCP von anderen
Rechnern** und gehört zum Team- oder Agency-Tarif. Er ist auch der Weg für Programme, die
sich noch nicht über einen Konnektor anmelden können (Cursor, Windsurf).

**Was du dafür brauchst**

1. **Team- oder Agency-Tarif** und eine laufende **SQL2REST**-Installation, in der der Schalter **„MCP für andere Rechner freigeben“** eingeschaltet ist.
2. Die **MCP-URL** und den **API-Key** deiner Installation. Beides findest du im
   **API-Dashboard → Einstellungen → Remote-Zugriff** (Tunnel-URL:
   `https://dein-tunnel.jtl2api.com/mcp`, im eigenen Netzwerk: `http://dein-server:8000/mcp`).
3. **Node.js 20 oder neuer** auf dem Rechner mit Claude Desktop. Prüfen mit `node -v`.
   Fehlt Node oder ist es älter als 20, von https://nodejs.org/en/download (LTS) installieren;
   unter Windows den Button **Windows Installer (.msi)**. Den Haken bei „Tools für native
   Module" (installiert Chocolatey) kannst du weglassen, `mcp-remote` braucht ihn nicht.

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
dem **API-Dashboard**:

```json
{
  "mcpServers": {
    "SQL2REST": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "https://dein-tunnel.jtl2api.com/mcp",
        "--transport", "http-only",
        "--header", "X-API-Key:${SQL2REST_KEY}"
      ],
      "env": { "SQL2REST_KEY": "DEIN-API-KEY" }
    }
  }
}
```

> - Der Key steht bewusst unten bei `env` und wird oben als `${SQL2REST_KEY}` referenziert —
>   so vermeiden wir einen Windows-Quoting-Fehler. **Kein Leerzeichen** nach `X-API-Key:`.
> - Die URL oben ist die SQL2REST-Tunnel-URL (HTTPS). Im eigenen Netzwerk: `http://dein-server:8000/mcp`.
>   Beide Adressen gibt es nur im Team- und Agency-Tarif.
> - **Wichtig:** Der `mcpServers`-Block muss **innerhalb** des vorhandenen `{ }`-Root-Objekts
>   deiner `claude_desktop_config.json` stehen — als Geschwister-Key neben anderen
>   Top-Level-Schlüsseln (z. B. `preferences`), **mit Komma davor**. Kein zweites `{ }`
>   dahinter anfügen — das ergibt kein gültiges JSON.
> - Hattest du vorher schon andere Server in der Datei, füge nur die
>   `"SQL2REST": { ... }`-Zeile innerhalb von `mcpServers` hinzu (Komma nicht vergessen).
> - **Als UTF-8 OHNE BOM speichern.** Sonst meldet Claude Desktop beim Start
>   *„Could not load app settings / Unexpected token … is not valid JSON"*.

Speichern und **Claude Desktop komplett neu starten** — nicht nur das Fenster schließen,
sondern über das **System-Tray → Rechtsklick → Beenden** ganz schließen, dann neu öffnen.
Beim ersten Start lädt `npx` den Helfer kurz im Hintergrund. Unter **Einstellungen →
Entwickler** sollte „SQL2REST" nach ein paar Sekunden als verbunden erscheinen.

### Projekt anlegen und Systemanweisung einfügen

Gilt für **alle drei Wege** und ist der eigentliche Zweck dieses Ordners.

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

**Konnektor (Weg A)**

- **Neue Werkzeuge tauchen im Chat nicht auf:** „Werkzeugliste aktualisieren" reicht
  **nicht**. Den Konnektor in Claude erst **trennen**, dann erscheint ein
  **Verbinden**-Knopf. Eine bestehende Verbindung behält die Datenbereiche, mit denen sie
  angelegt wurde.
- **„Neu verbinden nötig":** Erweitert dein Administrator deine Datenbereiche, während du
  schon verbunden bist, greift das erst nach einer neuen Verbindung. Die betroffene
  Verbindung sagt das selbst.
- **Anmeldung endet ohne Freigabe:** Ist MCP für diese Person nicht freigeschaltet, sagt
  die Freigabeseite das jetzt und nennt die betroffene Einstellung.
- **In Claude Desktop fehlt der Konnektor:** Er gehört zum Claude-Konto, nicht zum Gerät.
  Claude Desktop zeigt ihn nach einem Neustart.

**Konfigurationsdatei am Server selbst (Weg B)**

- **„sql2rest" erscheint nicht oder steht auf `failed`:** Stimmt der Pfad zu `sql2rest.exe`,
  mit doppelten Backslashes? Läuft der SQL2REST-Dienst? Auf gehosteten Windows-Servern startet
  Claude Desktop solche Einträge oft gar nicht, siehe den Hinweis bei Weg B.
- **JSON-Fehler beim Start:** dieselben Punkte wie bei Weg C (BOM, zweites `{ }`).

**Konfigurationsdatei von einem anderen Rechner (Weg C)**

- **„Could not load app settings / Unexpected token … is not valid JSON":** Die Datei wurde
  mit einem BOM gespeichert → als **UTF-8 ohne BOM** neu speichern (z.B. in VS Code unten rechts
  auf die Kodierung klicken → „Save with Encoding" → „UTF-8").
- **„SQL2REST: Server disconnected" / Status `failed`:** Fast immer **Node zu alt oder gar nicht
  installiert**. `mcp-remote` braucht **Node 20+**; unter Node 18 stürzt es mit
  `undici: File is not defined` ab. `node -v` prüfen und auf 20+ aktualisieren.
- **Node gerade erst installiert?** Claude Desktop übernimmt die Umgebung (den `PATH`) beim
  Start. Ist die App zum Zeitpunkt der Node-Installation schon gelaufen, findet sie das neue
  Node nicht — sie meldet weiter `failed`, obwohl `node -v` im Terminal längst funktioniert.
  Danach die App **komplett beenden** und neu starten.
- **Kein nvm für Claude Desktop.** Claude Desktop startet als normales Programm und erbt die
  nvm-Umgebung deiner Shell **nicht** (Windows wie Linux). Installiere Node am einfachsten über
  den normalen Installer von [nodejs.org](https://nodejs.org/en/download) (LTS). Wenn du bei nvm
  bleiben willst: Standard-Node auf 20+ setzen und Desktop neu starten, oder den vollen Pfad zur
  `npx` aus deiner Node-20-Installation in `command` eintragen und `env.PATH` entsprechend setzen.
- **Änderung wirkt nicht:** Nur das Fenster zu schließen reicht nicht — die App läuft im Tray
  weiter. Immer **komplett beenden** und neu starten.
- **Der native `type: "http"`-Block:** Manche älteren Claude-Desktop-Builds nahmen einen
  direkten `{ "type": "http", "url": …, "headers": { "X-API-Key": … } }`-Eintrag an.
  Aktuelle Builds (geprüft 1.24012.9, August 2026) **entfernen ihn stillschweigend** und
  verbinden nie — nimm den `mcp-remote`-Block oben.

### Mehrere Mitarbeiter (Team-Tarif)

**Mit Konnektor:** Du lädst jede Person im Reiter **„Schlüssel"** ein und weist ihr die
Datenbereiche zu, die sie sehen darf. Jede Person meldet sich dann mit ihrem **eigenen**
SQL2REST-Login an; es wird kein Schlüssel weitergegeben, und jede Verbindung lässt sich
einzeln trennen.

**Mit Konfigurationsdatei (Weg C):** Jeder Mitarbeiter trägt den Block einmal in **seine eigene**
`claude_desktop_config.json` ein.

In beiden Fällen legt jede Person ein Projekt mit demselben Template an. Der API-Key liegt
nie im Template — das Template kann also unbedenklich geteilt werden.

---

## English

### Route A — connector (Team/Agency plan, recommended)

Add SQL2REST in Claude as a custom connector, sign in, done. No configuration file, no
extra process on the machine, no API key to paste anywhere. It works in the browser on
claude.ai, in Claude Desktop, and in the Claude apps on phone and tablet.

**What you need**

1. **SQL2REST v1.4.0 or newer**, running on the server where your JTL-Wawi lives.
2. **An HTTPS tunnel.** Built in since v1.3.29, there is nothing extra to install.
3. **Team or Agency plan.** The connector needs a public address, and that starts with the
   Team plan. On the Personal plan and during the trial, use **route B**.

**Connected in five steps**

1. In Claude go to **Settings → "Customize" → "Connectors"**. Top right under **"Add"**,
   choose **"Add custom connector"**.
2. **Enter the address of your system:** your public address ending in `/mcp`, with no
   trailing slash — e.g. `https://your-tunnel.jtl2api.com/mcp`. The address is in your
   **member portal on sql2rest.com under "Your addresses"**, in the MCP row, and in the
   API Dashboard under **Settings → Remote Access**. One click on copy and you have it.
3. **Start the connection.** You land on the SQL2REST sign-in and get a magic link by
   email. You do not need a password.
4. **Approve.** Before you confirm you see which account this is, which system is being
   connected and which data areas are being shared.
5. **Done.** The connector is in your list. It belongs to your Claude account, so it works
   in the browser and on your phone too. In Claude Desktop it shows up after a restart.

**You decide what Claude may read.** Sign-in runs over OAuth, the way you know it from
"Sign in with Google": Claude receives no key, only its own per-person grant that you can
withdraw individually at any time. What gets granted is always the intersection of what
your administrator assigned to you and what the app requested. The **"Keys"** tab holds
people, their connected apps and their keys all in one place; any single connection can be
disconnected without taking away the access of that person.

Continue below at **"Create a project and paste the instructions"**.

### Route B — configuration file on the server itself (every plan)

Claude Desktop, Claude Code, Cursor or Windsurf run directly on the server where SQL2REST
runs. The program then starts SQL2REST itself, with no address and no network. This works
on every plan, and on the Personal plan and during the trial it is the only route: MCP from
other computers, whether from your own network or from the internet, is part of the Team
plan.

> **A point that surprises many people:** the configuration file starts the MCP server in a
> local session on the machine Claude runs on. On hosted Windows servers, which is where
> the JTL-Wawi usually lives, that session often cannot start at all. From the Team plan
> on, route A or route C is then the way.

**What you need**

1. A running **SQL2REST** install (Windows service) on this server.
2. The path to `sql2rest.exe`, usually `C:\sql2rest\sql2rest.exe`. You need no switch in the
   API Dashboard, no API key and no Node.js: SQL2REST reads the key from its own configuration.

**Step 1 — open the config file** as described under route C below (in Claude Desktop:
**Settings → Developer → "Edit Config"**).

**Step 2 — paste this block**

```json
{
  "mcpServers": {
    "sql2rest": {
      "command": "C:\\sql2rest\\sql2rest.exe",
      "args": ["--mcp"]
    }
  }
}
```

> - If SQL2REST lives elsewhere, adjust the path. Backslashes are doubled inside JSON.
> - As with route C: the `mcpServers` block goes **inside** the existing root `{ }`, with a
>   comma before it, and the file is saved as **UTF-8 without a BOM**.
> - Cursor and Windsurf take the same block in their own MCP configuration.

Save and **fully quit Claude Desktop** (system tray → right-click → Quit), then reopen.
"sql2rest" should then appear as connected under **Settings → Developer**.

### Route C — configuration file on another computer (Team/Agency plan)

Claude Desktop, Claude Code, Cursor or Windsurf run on a different computer than SQL2REST,
on your own network or anywhere on the internet. This route is **MCP from other computers**
and is part of the Team or Agency plan. It is also the route for clients that cannot sign in
through a connector yet (Cursor, Windsurf).

**What you need**

1. **Team or Agency plan** and a running **SQL2REST** install with the switch **“Allow MCP from other computers”** turned on.
2. Your install's **MCP URL** and **API key** (**API Dashboard → Settings → Remote Access**;
   Tunnel URL: `https://your-tunnel.jtl2api.com/mcp`; on your own network: `http://your-server:8000/mcp`).
3. **Node.js 20 or newer** on the machine running Claude Desktop. Check with `node -v`;
   install the LTS from https://nodejs.org/en/download if missing or older than 20; on Windows
   use the **Windows Installer (.msi)** button. You can leave the "Tools for Native Modules"
   checkbox (which installs Chocolatey) unticked — `mcp-remote` does not need it.

**Step 1 — open the config file**

**Settings → Developer → "Edit Config"** opens `claude_desktop_config.json`. Manually:
- **Windows:** `%APPDATA%\Claude\`
- **macOS:** `~/Library/Application Support/Claude/`
- **Linux:** `~/.config/Claude/`

If the file is empty, put `{}` in it.

**Step 2 — paste this block**

Add the `mcpServers` block and replace **URL** and **API key** with the values from the
**API Dashboard**:

```json
{
  "mcpServers": {
    "SQL2REST": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "https://dein-tunnel.jtl2api.com/mcp",
        "--transport", "http-only",
        "--header", "X-API-Key:${SQL2REST_KEY}"
      ],
      "env": { "SQL2REST_KEY": "YOUR-API-KEY" }
    }
  }
}
```

> - The key stays in `env` (referenced as `${SQL2REST_KEY}`) to avoid Windows quoting issues.
>   **No space** after `X-API-Key:`.
> - The URL above is the SQL2REST Tunnel URL (HTTPS). On your own network: `http://your-server:8000/mcp`.
>   Both addresses exist only on the Team and Agency plans.
> - **Important:** paste the `mcpServers` block **inside** the existing root `{ }` of
>   `claude_desktop_config.json` — as a sibling of other top-level keys (e.g. `preferences`),
>   **with a comma before it**. Do NOT paste it as a second standalone `{ }` object — that
>   produces invalid JSON ("Unexpected non-whitespace character after JSON").
> - Already had other servers? Just add the `"SQL2REST": { ... }` entry inside `mcpServers`.
> - **Save as UTF-8 WITHOUT a BOM**, or Claude Desktop shows *"Could not load app settings /
>   Unexpected token … is not valid JSON"* on launch.

Save and **fully quit Claude Desktop** — not just the window; **system tray → right-click →
Quit** — then reopen. On first run `npx` fetches the bridge briefly. "SQL2REST" should appear
as connected under **Settings → Developer** after a few seconds.

### Create a project and paste the instructions

Applies to **all three routes**, and it is what this folder is actually for.

Create a project, e.g. "JTL-Wawi (SQL2REST)", and copy the **entire**
[`system-instructions.en.md`](./system-instructions.en.md) into the project's **Custom
Instructions**. Done.

### Test it

- "What were the last 3 orders?"
- "Show me the top 10 customers by revenue in May 2026."
- "Stock for SKU ABC-123?"

### Troubleshooting

**Connector (route A)**

- **New tools do not appear in chat:** "Update tools list" is **not enough**. First
  **"Disconnect"** the connector in Claude, then a **"Connect"** button appears. An existing
  connection keeps the data areas it was created with.
- **"Reconnect needed":** if your administrator widens your data areas while you are already
  connected, it only takes effect after you reconnect. The affected connection says so itself.
- **Sign-in ends without a consent screen:** if MCP is not enabled for that person, the
  consent screen now says so and names the setting in question.
- **The connector is missing in Claude Desktop:** it belongs to the Claude account, not the
  device. Claude Desktop shows it after a restart.

**Configuration file on the server itself (route B)**

- **"sql2rest" does not appear or shows `failed`:** is the path to `sql2rest.exe` right, with
  doubled backslashes? Is the SQL2REST service running? On hosted Windows servers Claude
  Desktop often cannot start such entries at all, see the note under route B.
- **JSON error on launch:** the same points as for route C (BOM, a second `{ }`).

**Configuration file on another computer (route C)**

- **"Could not load app settings / Unexpected token … is not valid JSON":** the file was saved
  with a BOM → re-save as **UTF-8 without BOM** (i.e. VS Code: click the encoding at bottom-right →
  "Save with Encoding" → "UTF-8").
- **"SQL2REST: Server disconnected" / status `failed`:** almost always **Node too old, or not
  installed at all**. `mcp-remote` needs **Node 20+**; on Node 18 it crashes with
  `undici: File is not defined`. Check `node -v`.
- **Just installed Node?** Claude Desktop picks up its environment (the `PATH`) at launch. If the
  app was already running when you installed Node, it will not see the new Node and keeps
  reporting `failed`, even though `node -v` works fine in a terminal. **Fully quit** the app and
  start it again.
- **No nvm for Claude Desktop.** Claude Desktop launches as a normal application and does **not**
  inherit your shell's nvm environment (Windows and Linux alike). The simplest route is the plain
  installer from [nodejs.org](https://nodejs.org/en/download) (LTS). If you want to stay on nvm:
  set the default Node to 20+ and relaunch Desktop, or pin the absolute `npx` path from your
  Node 20 install in `command` and set `env.PATH` to that bin directory.
- **Change didn't apply:** closing the window isn't enough (app stays in the tray). **Fully
  quit** and restart.
- **The native `type: "http"` block:** some older Claude Desktop builds accepted a direct
  `{ "type": "http", "url": …, "headers": { "X-API-Key": … } }` entry. Current builds
  (verified 1.24012.9, Aug 2026) **silently strip it** and never connect — use the
  `mcp-remote` block above.

### Multiple team members (Team plan)

**With the connector:** invite each person in the **"Keys"** tab and assign the data areas
they may see. Each person then signs in with their **own** SQL2REST login; no key is handed
around, and any single connection can be disconnected on its own.

**With the configuration file (route C):** each person adds the block once to **their own**
`claude_desktop_config.json`.

Either way, each person creates a project with the same template. The API key never lives in
the template — so the template is safe to share.

---

## Files

| File | Purpose |
|------|---------|
| [`system-instructions.de.md`](./system-instructions.de.md) | German system instructions — paste into the project |
| [`system-instructions.en.md`](./system-instructions.en.md) | English system instructions |
| `README.md` | This setup guide |

The instruction mentions a `tool-reference` file next to it. That file is part of
your SQL2REST installation (folder `ai-project-template` next to the exe, called
`claude-desktop-template` on older builds) and
is not published here. You do not need it to use the template: your AI app already
sees every tool and every parameter in its own tool list.

## Notes

- The template is intentionally **provider-agnostic about your data** — no customer names,
  SKUs, or numbers are hardcoded. It describes *how* SQL2REST works, not *what* is in your DB.
- Keep the template in sync with the API: the tool list mirrors SQL2REST's MCP tools. If a
  future release adds tools, update the "Available tools" section.
- Over a connector, Claude only sees the tools that fall inside the data areas the person was
  granted. A shorter tool list than this template describes is that filter working, not a bug.
- Full API reference: Swagger UI at `/docs` on your SQL2REST server.
