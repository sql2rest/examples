# SQL2REST make Blueprint Templates

Importierbare make-Blueprints fur deine JTL-Wawi-Daten. Alle Blueprints lesen Daten
uber die SQL2REST REST-API und authentifizieren sich per `X-API-Key` Header.

| Datei | Beschreibung / Description |
|---|---|
| `sql2rest-starter.blueprint.json` | Generischer Startpunkt: ein HTTP-GET-Modul. Passe den Endpoint-Pfad beliebig an. / Generic starter: one HTTP GET module. Change the endpoint path freely. |
| `sql2rest-orders-notification.blueprint.json` | Bestellungen abrufen, Variable setzen, eigenes Ziel einbinden. / Fetch orders, set variable, connect your destination. |

---

> **Sicherheitshinweis:** make speichert den API-Key DIREKT in der Blueprint-Datei
> (im `headers`-Feld des HTTP-Moduls), nicht in einem separaten Credential-Store.
> Die Vorlagedateien enthalten ausschliesslich den Platzhalter `YOUR-API-KEY`.
> **Teile die ausgefullte Blueprint-Datei NICHT** (nicht per E-Mail, Git, Chat o. a.),
> sobald du den echten Key eingetragen hast. Behandle sie wie ein Passwort.

> **Security notice:** make stores the API key DIRECTLY in the blueprint file
> (in the HTTP module's `headers` field), not in a separate credential store.
> The template files contain only the placeholder `YOUR-API-KEY`.
> **Do NOT share the filled-in blueprint file** (not via email, Git, chat, etc.)
> once you have entered your real key. Treat it like a password.

---

## Deutsch

### Was du brauchst

1. Eine laufende **SQL2REST**-Installation (v1.2+).
2. Einen **API-Key** aus dem SQL2REST API-Dashboard.
3. Einen aktiven **make**-Account (kostenlose Stufe genugt fur den Einstieg).

### Schritt 1: Blueprint importieren

1. Lade eine der `.json`-Dateien aus diesem Ordner herunter.
2. Offne den **make-Szenario-Editor**.
3. Klicke oben rechts auf das **Dreipunkt-Menu** (drei Punkte).
4. Wahle **"Import Blueprint"**.
5. Lade die heruntergeladene `.json`-Datei hoch und bestatige.

### Schritt 2: Platzhalter ersetzen

Nach dem Import enthalt das HTTP-Modul zwei Platzhalter, die du ersetzen musst:

**`YOUR-SERVER:8000` ersetzen** (URL-Feld im HTTP-Modul):

- Lokales Netz: `http://192.168.1.10:8000`
- Hostname: `http://mein-server.local:8000`
- Mit HTTPS (Reverse-Proxy): `https://api.mein-shop.de`

**`YOUR-API-KEY` ersetzen** (Header-Wert `X-API-Key` im HTTP-Modul):

1. HTTP-Modul im Szenario-Editor anklicken.
2. Im Abschnitt "Headers" den Wert des Headers `X-API-Key` anklicken.
3. `YOUR-API-KEY` durch deinen echten Key aus dem SQL2REST API-Dashboard ersetzen.
4. Szenario speichern.

> **Achtung:** Speichere und teile die Blueprint-Datei NICHT erneut, nachdem du den
> echten Key eingetragen hast. Der Key ist dann in der JSON-Datei sichtbar.

### Schritt 3: Ziel-Modul einbinden (nur Orders-Notification)

Das `sql2rest-orders-notification.blueprint.json` enthalt als zweites Modul einen
`util:SetVariables`-Platzhalter. Ersetze dieses Modul durch deine gewunschte
Aktion (z. B. Slack-Nachricht, E-Mail via Gmail, HTTP-Webhook an ein anderes System).

### Erreichbarkeit

**make-Cloud (Standard) -- SQL2REST muss offentlich erreichbar sein:**

make fuhrt Szenarien aus seinen Cloud-Servern aus. `http://localhost:8000` oder eine
reine LAN-IP funktionieren deshalb NICHT direkt.

Nutze einen Reverse-Proxy (z. B. Cloudflare Tunnel, Caddy), um SQL2REST erreichbar
zu machen. Anleitung: https://sql2rest.com/docs/reverse-proxy/

**make On-Premise (lokale Ausfuhrung):**

Falls du make lokal betreibst und im selben Netz wie die Wawi-Box bist, kann
`http://localhost:8000` oder die LAN-IP direkt verwendet werden.

### Verfugbare Endpunkte

| Endpoint | Beschreibung |
|---|---|
| `/orders` | Bestellungen |
| `/products` | Produkte |
| `/customers` | Kunden |
| `/stock/by-warehouse` | Lagerbestand je Lager |
| `/warehouses` | Lager |
| `/sync/orders` | Bestellungen (optimiert fur CRM-Sync) |
| `/sync/customers` | Kunden (optimiert fur CRM-Sync) |

Vollstandige API-Referenz: Swagger-UI unter `/docs` auf deinem SQL2REST-Server.

---

## English

### What you need

1. A running **SQL2REST** install (v1.2+).
2. An **API key** from the SQL2REST API Dashboard.
3. An active **make** account (free tier is sufficient to get started).

### Step 1: Import a blueprint

1. Download one of the `.json` files from this folder.
2. Open the **make Scenario Editor**.
3. Click the **three-dot menu** in the top right corner.
4. Select **"Import Blueprint"**.
5. Upload the downloaded `.json` file and confirm.

### Step 2: Replace the placeholders

After import the HTTP module contains two placeholders you must replace:

**Replace `YOUR-SERVER:8000`** (URL field in the HTTP module):

- Local network: `http://192.168.1.10:8000`
- Hostname: `http://my-server.local:8000`
- With HTTPS (reverse proxy): `https://api.my-shop.com`

**Replace `YOUR-API-KEY`** (header value for `X-API-Key` in the HTTP module):

1. Click the HTTP module in the Scenario Editor.
2. In the "Headers" section, click the value of the `X-API-Key` header.
3. Replace `YOUR-API-KEY` with your real key from the SQL2REST API Dashboard.
4. Save the scenario.

> **Warning:** Do NOT save and share the blueprint file again after entering your
> real key. The key will be visible in plain text inside the JSON file.

### Step 3: Connect a destination module (Orders Notification only)

The `sql2rest-orders-notification.blueprint.json` includes a `util:SetVariables`
placeholder as the second module. Replace it with your desired action (e.g. Slack
message, email via Gmail, HTTP webhook to another system).

### Reachability

**make Cloud (standard) -- SQL2REST must be publicly reachable:**

make runs scenarios from its cloud servers. `http://localhost:8000` or a LAN-only IP
does NOT work directly from make Cloud.

Use a reverse proxy (e.g. Cloudflare Tunnel, Caddy) to expose SQL2REST publicly.
Guide: https://sql2rest.com/docs/reverse-proxy/

**make On-Premise (local execution):**

If you run make locally on the same network as the Wawi box, `http://localhost:8000`
or the LAN IP can be used directly.

### Available endpoints

| Endpoint | Description |
|---|---|
| `/orders` | Orders |
| `/products` | Products |
| `/customers` | Customers |
| `/stock/by-warehouse` | Stock by warehouse |
| `/warehouses` | Warehouses |
| `/sync/orders` | Orders (optimized for CRM sync) |
| `/sync/customers` | Customers (optimized for CRM sync) |

Full API reference: Swagger UI at `/docs` on your SQL2REST server.

---

## Files

| File | Purpose |
|---|---|
| `sql2rest-starter.blueprint.json` | Generic "call any endpoint" starter |
| `sql2rest-orders-notification.blueprint.json` | Fetch orders and forward to notification destination |
| `README.md` | This guide |

## Notes

- Templates are **endpoint-agnostic**: no customer data, SKUs, or real keys are embedded.
- The `X-API-Key` header name matches the SQL2REST default. If you changed `HeaderName`
  in your `config.ini`, update the header name in the HTTP module accordingly.
- Unlike n8n, make does not have a separate credential store for generic HTTP requests.
  The API key lives inline in the blueprint JSON -- handle it accordingly.
- Delta sync (`?changed_since=`) is not yet implemented in SQL2REST v1.x. For
  change-based automation, use a full pull and filter by date in your make scenario.
