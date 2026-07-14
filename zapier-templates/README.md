# SQL2REST Zapier Templates

Verbinde deine JTL-Wawi-Daten mit Zapier uber "Webhooks by Zapier". SQL2REST stellt
eine vollstandige REST-API bereit; Zapier ruft sie per HTTP-GET-Aktion mit einem
`X-API-Key` Header ab. Kein eigener Zapier-App-Review notwendig.

> **Kein Download:** Zapier bietet fur diesen Anwendungsfall keine herunterladbare
> Vorlagen-Datei. Folge der manuellen Schritt-fur-Schritt-Anleitung unten.

---

## Deutsch

### Was du brauchst

1. Eine laufende **SQL2REST**-Installation (v1.2+).
2. Einen **API-Key** aus dem SQL2REST API-Dashboard.
3. Einen **Zapier-Account** (Webhooks by Zapier erfordert einen kostenpflichtigen
   Zapier-Tarif; kostenlose Legacy-Accounts konnen Zaps mit Webhooks nicht teilen).

### Beispiel-Anwendungsfall: Neue Bestellungen abfragen

Dieses Beispiel ruft alle 15 Minuten die Bestellungen von SQL2REST ab und leitet sie
an deine gewunschte App weiter (z. B. Slack, Gmail, Google Sheets).

### Schritt 1: Trigger anlegen (Schedule by Zapier)

1. Neuen Zap erstellen.
2. Als Trigger **"Schedule by Zapier"** wahlen.
3. Intervall einstellen, z. B. **"Every 15 Minutes"**.

### Schritt 2: Aktion "Webhooks by Zapier" hinzufugen

1. Neue Aktion hinzufugen.
2. App suchen: **"Webhooks by Zapier"**.
3. Aktion wahlen: **"GET"**.
4. Konfigurieren:
   - **URL:** `http://YOUR-SERVER:8000/orders`
     (Ersetze `YOUR-SERVER:8000` durch deine SQL2REST-Adresse; siehe Abschnitt "Erreichbarkeit")
   - **Payload Type:** leer lassen (kein Body bei GET)
   - **Headers:** Schlussel-Wert-Paar hinzufugen:
     - Key: `X-API-Key`
     - Value: dein API-Key aus dem SQL2REST API-Dashboard

   > Trage hier deinen echten API-Key ein. Er wird sicher in deinem Zapier-Account
   > gespeichert.

5. Verbindung testen und bestatigen.

### Schritt 3: Ziel-Aktion hinzufugen

Fuge eine weitere Aktion hinzu (z. B. "Slack: Send Message", "Gmail: Send Email",
"Google Sheets: Create Row") und mappe die Felder aus der Webhooks-Antwort.

### Erreichbarkeit

**Zapier-Cloud (Standard) -- SQL2REST muss offentlich erreichbar sein:**

Zapier fuhrt Zaps aus seinen Cloud-Servern aus. `http://localhost:8000` oder eine
reine LAN-IP funktionieren deshalb NICHT.

Nutze einen Reverse-Proxy (z. B. Cloudflare Tunnel, Caddy), um SQL2REST erreichbar
zu machen. Anleitung: https://sql2rest.com/docs/reverse-proxy/

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
3. A **Zapier account** (Webhooks by Zapier requires a paid Zapier plan; free Legacy
   accounts cannot share Zaps that use Webhooks).

### Example use case: Poll new orders

This example polls orders from SQL2REST every 15 minutes and forwards them to an app
of your choice (e.g. Slack, Gmail, Google Sheets).

### Step 1: Create a trigger (Schedule by Zapier)

1. Create a new Zap.
2. Choose **"Schedule by Zapier"** as the trigger.
3. Set the interval, e.g. **"Every 15 Minutes"**.

### Step 2: Add a "Webhooks by Zapier" action

1. Add a new action.
2. Search for app: **"Webhooks by Zapier"**.
3. Choose action: **"GET"**.
4. Configure:
   - **URL:** `http://YOUR-SERVER:8000/orders`
     (Replace `YOUR-SERVER:8000` with your SQL2REST address; see "Reachability" below)
   - **Payload Type:** leave empty (no body for GET)
   - **Headers:** add a key-value pair:
     - Key: `X-API-Key`
     - Value: your API key from the SQL2REST API Dashboard

   > Enter your real API key here. It is stored securely in your Zapier account.

5. Test the connection and confirm.

### Step 3: Add a destination action

Add another action (e.g. "Slack: Send Message", "Gmail: Send Email",
"Google Sheets: Create Row") and map fields from the Webhooks response.

### Reachability

**Zapier Cloud (standard) -- SQL2REST must be publicly reachable:**

Zapier runs Zaps from its cloud servers. `http://localhost:8000` or a LAN-only IP
does NOT work.

Use a reverse proxy (e.g. Cloudflare Tunnel, Caddy) to expose SQL2REST publicly.
Guide: https://sql2rest.com/docs/reverse-proxy/

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

## Notes

- Zapier does not use a downloadable JSON file for Zap sharing -- templates are
  URL-based links, not file artifacts.
- The `X-API-Key` header name matches the SQL2REST default. If you changed
  `HeaderName` in your `config.ini`, update the header key in the Webhooks action.
- Delta sync (`?changed_since=`) is not yet implemented in SQL2REST v1.x. For
  change-based automation, use a full pull and filter by date inside your Zap.
- "Webhooks by Zapier" is available on all paid Zapier plans. Free-plan users can
  use the Webhooks step as an action in a Zap (triggered by another app event) but
  not as a polling trigger.
