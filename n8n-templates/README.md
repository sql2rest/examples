# SQL2REST n8n Workflow Templates

Importierbare n8n-Workflows fur deine JTL-Wawi-Daten. Alle Workflows lesen Daten
uber die SQL2REST REST-API und authentifizieren sich per `X-API-Key` Header Auth.

| Datei | Beschreibung / Description |
|---|---|
| `sql2rest-starter.workflow.json` | Generischer Startpunkt: ein Trigger + ein HTTP-Request-Node. Passe den Endpoint-Pfad beliebig an. / Generic starter: one trigger + one HTTP Request node. Change the endpoint path freely. |
| `sql2rest-orders-notification.workflow.json` | Bestellungen alle 15 Minuten abrufen, Duplikate entfernen, Ziel-Node einbinden. / Poll orders every 15 min, deduplicate, connect your destination. |
| `sql2rest-product-export.workflow.json` | Taglich Produkte exportieren (z. B. Google Sheets oder CSV). / Daily product export (e.g. Google Sheets or CSV). |
| `sql2rest-low-stock-alert.workflow.json` | Lagerbestand alle 4 Stunden prufen, Artikel unter Schwellenwert weiterleiten. Plattformunabhangig. / Check stock every 4 hours, forward items below threshold. Platform-neutral. |

---

## Deutsch

### Was du brauchst

1. Eine laufende **SQL2REST**-Installation (v1.2+).
2. Einen **API-Key** aus dem SQL2REST API-Dashboard.
3. Eine laufende **n8n**-Instanz (Self-hosted oder n8n Cloud).

### Schritt 1: Credential anlegen

Erstelle in n8n unter **Credentials -> New** ein neues Credential vom Typ **Header Auth**:

- **Name** des Credentials (frei wahlbar): `SQL2REST API Key`
- Feld **Name** (Header-Name): `X-API-Key`
- Feld **Value** (Header-Wert): dein Key aus dem SQL2REST API-Dashboard

> Dieses Credential enthalt deinen geheimen Key. Teile es nicht. Der Key ist
> instanzgebunden -- nach dem Import musst du das Credential im Workflow neu verbinden.

### Schritt 2: Workflow importieren

**Variante A -- Datei hochladen:**

1. n8n-Editor offnen.
2. Oben im Menu: **Import from File** wahlen.
3. Eine der `.json`-Dateien aus diesem Ordner auswahlen und bestatigen.

**Variante B -- URL importieren (direkter GitHub-Link):**

1. n8n-Editor offnen.
2. Oben im Menu: **Import from URL** wahlen.
3. Folgende URL einfugen (Beispiel fur den Starter):

```
https://raw.githubusercontent.com/sql2rest/examples/main/n8n/sql2rest-starter.workflow.json
```

Weitere URLs:
- `https://raw.githubusercontent.com/sql2rest/examples/main/n8n/sql2rest-orders-notification.workflow.json`
- `https://raw.githubusercontent.com/sql2rest/examples/main/n8n/sql2rest-product-export.workflow.json`
- `https://raw.githubusercontent.com/sql2rest/examples/main/n8n/sql2rest-low-stock-alert.workflow.json`

### Schritt 3: Credential verbinden

Nach dem Import erscheint am HTTP-Request-Node ein **orangefarbener Hinweis** ("Credential not found").
Das ist normal -- Credential-IDs sind instanzspezifisch.

1. HTTP-Request-Node anklicken.
2. Im Credential-Dropdown **"SQL2REST API Key"** auswahlen (das in Schritt 1 angelegte Credential).
3. Speichern.

### Schritt 4: Basis-URL ersetzen

Jeder Workflow enthalt den Platzhalter `http://YOUR-SERVER:8000`.
Ersetze ihn durch die Adresse deines SQL2REST-Servers, z. B.:

- Lokales Netz: `http://192.168.1.10:8000`
- Hostname: `http://mein-server.local:8000`
- Mit HTTPS (Reverse-Proxy): `https://api.mein-shop.de`

### Erreichbarkeit

**Self-hosted n8n im selben Netzwerk wie die Wawi-Box:**
`http://localhost:8000` oder die LAN-IP funktionieren direkt.

**n8n Cloud:**
SQL2REST muss offentlich erreichbar sein. Nutze dafur einen Reverse-Proxy
(z. B. Cloudflare Tunnel, Caddy). Anleitung: https://sql2rest.com/docs/reverse-proxy/

### Hinweis zur typeVersion

Die Workflows sind mit `typeVersion: 4.2` fur den HTTP-Request-Node ausgeliefert.
Wenn n8n beim Import eine Warnung anzeigt, prufe die `typeVersion` deiner n8n-Version
und passe sie im Node-Editor an. Betrifft nur altere oder neuere n8n-Instanzen.

### Verfugbare Endpunkte

Alle SQL2REST-Endpunkte konnen im Starter-Workflow verwendet werden:

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
3. A running **n8n** instance (self-hosted or n8n Cloud).

### Step 1: Create a credential

In n8n, go to **Credentials -> New** and create a credential of type **Header Auth**:

- **Credential name** (your choice): `SQL2REST API Key`
- Field **Name** (header name): `X-API-Key`
- Field **Value** (header value): your key from the SQL2REST API Dashboard

> This credential holds your secret key. Do not share it. Credential IDs are
> instance-specific -- you must reconnect the credential after import.

### Step 2: Import a workflow

**Option A -- upload a file:**

1. Open the n8n Editor.
2. Use the top menu: **Import from File**.
3. Select one of the `.json` files from this folder and confirm.

**Option B -- import from URL (direct GitHub link):**

1. Open the n8n Editor.
2. Use the top menu: **Import from URL**.
3. Paste the URL (example for the starter):

```
https://raw.githubusercontent.com/sql2rest/examples/main/n8n/sql2rest-starter.workflow.json
```

Other URLs:
- `https://raw.githubusercontent.com/sql2rest/examples/main/n8n/sql2rest-orders-notification.workflow.json`
- `https://raw.githubusercontent.com/sql2rest/examples/main/n8n/sql2rest-product-export.workflow.json`
- `https://raw.githubusercontent.com/sql2rest/examples/main/n8n/sql2rest-low-stock-alert.workflow.json`

### Step 3: Reconnect the credential

After import, the HTTP Request node shows an **orange warning** ("Credential not found").
This is expected -- credential IDs are instance-specific.

1. Click the HTTP Request node.
2. In the credential dropdown, select **"SQL2REST API Key"** (the one you created in Step 1).
3. Save.

### Step 4: Replace the base URL

Each workflow uses the placeholder `http://YOUR-SERVER:8000`.
Replace it with your SQL2REST server address, e.g.:

- Local network: `http://192.168.1.10:8000`
- Hostname: `http://my-server.local:8000`
- With HTTPS (reverse proxy): `https://api.my-shop.com`

### Reachability

**Self-hosted n8n on the same LAN as the Wawi box:**
`http://localhost:8000` or the LAN IP works directly.

**n8n Cloud:**
SQL2REST must be publicly reachable. Use a reverse proxy
(e.g. Cloudflare Tunnel, Caddy). Guide: https://sql2rest.com/docs/reverse-proxy/

### Note on typeVersion

The workflows ship with `typeVersion: 4.2` for the HTTP Request node.
If n8n shows a warning on import, check the typeVersion for your n8n version
and update it in the node editor. This only affects older or newer n8n instances.

### Available endpoints

All SQL2REST endpoints can be used in the starter workflow:

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
| `sql2rest-starter.workflow.json` | Generic "call any endpoint" starter |
| `sql2rest-orders-notification.workflow.json` | Poll orders, deduplicate, notify |
| `sql2rest-product-export.workflow.json` | Daily scheduled product export |
| `sql2rest-low-stock-alert.workflow.json` | Low-stock threshold alert (platform-neutral) |
| `README.md` | This guide |

## Notes

- Templates are **endpoint-agnostic**: no customer data, SKUs, or real keys are embedded.
- The `X-API-Key` header name matches the SQL2REST default. If you changed `HeaderName`
  in your `config.ini`, update the credential accordingly.
- Delta sync (`?changed_since=`) is not yet implemented in SQL2REST v1.x. These templates
  use full-pull + deduplication in n8n (`Remove Duplicates` node) as a workaround.
- For n8n public template gallery submission (D-13), see the project roadmap.
