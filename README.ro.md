<p align="center">
  <img src="assets/banner-1200x200.png" alt="Vendwiser: magazinele tale eMAG și Trendyol, conduse din asistentul AI" width="100%">
</p>

<h1 align="center">Serverul MCP Vendwiser</h1>

<p align="center">
  <strong>Conduci magazinele de pe eMAG și Trendyol întrebând: oferte, pagini de produs, comenzi, retururi, întrebările clienților, facturi, plus AWB-uri și campanii pe eMAG, cu aprobarea ta la fiecare schimbare.</strong>
</p>

<p align="center">
  <a href="https://vendwiser.ai">Site</a> ·
  <a href="https://vendwiser.ai/docs/">Documentație</a> ·
  <a href="docs/tools.md">Toate cele 52 de unelte (EN)</a> ·
  <a href="docs/security.md">Securitate (EN)</a> ·
  <a href="README.md">English</a>
</p>

[![smithery badge](https://smithery.ai/badge/sergiu/vendwiser)](https://smithery.ai/servers/sergiu/vendwiser)

<!-- Insigne directoare: decomentează după ce listările există și înlocuiește valorile de rezervă.
[![Vendwiser on Glama](https://glama.ai/mcp/connectors/GLAMA_CONNECTOR_ID/badge)](https://glama.ai/mcp/connectors/GLAMA_CONNECTOR_ID)
-->

> **Acest depozit conține doar documentație și fișiere de configurare pentru clienți.** Serverul Vendwiser este un serviciu găzduit, proprietar, operat de VOLTIVO POWER TRADING SRL. Nu ai nimic de compilat sau de rulat local: îți conectezi clientul AI la adresa de mai jos.

| | |
|---|---|
| **Adresa serverului** | `https://app.vendwiser.ai/mcp` |
| **Transport** | Streamable HTTP (fără stare) |
| **Autentificare** | OAuth 2.1 cu înregistrare dinamică a clientului și PKCE (S256). Te autentifici în Vendwiser într-o fereastră de browser; nu copiezi nicio cheie API. |
| **Unelte** | 52 (33 doar de citire, 17 care doar *propun* schimbări, 1 router, 1 care aplică schimbările aprobate) |
| **Marketplace-uri** | eMAG Marketplace România. Trendyol România, Bulgaria, Grecia, Cehia, Slovacia și Germania (fiecare magazin conectat separat). Oblio pentru facturare (opțional). |
| **Nume în registru** | `ai.vendwiser/vendwiser` |

## Ce este

Vendwiser îți conectează conturile de vânzător **eMAG** și **Trendyol** și **contul de facturare Oblio** la asistentul AI pe care îl folosești deja (Claude, ChatGPT, Cursor, VS Code, Gemini CLI și orice alt client MCP), ca să faci munca zilnică de marketplace în limbaj obișnuit.

Întrebi ce comenzi au depășit termenul de predare, de ce a fost respinsă o ofertă, ce produse au pierdut butonul de cumpărare, ce întrebări ale clienților așteaptă răspuns sau cât ți-a plătit Trendyol săptămâna trecută. Când vrei să schimbi ceva, asistentul pregătește schimbarea, îți arată o previzualizare înainte și după, și așteaptă aprobarea ta explicită.

## Pentru cine este

- **Vânzătorii de pe eMAG Marketplace România** care vor răspunsuri și schimbări de rutină fără să navigheze prin Seller Center.
- **Vânzătorii Trendyol** din România, Bulgaria, Grecia, Cehia, Slovacia și Germania care vor să gestioneze comenzile, paginile de produs, retururile și întrebările clienților dintr-o conversație.
- **Operatorii de magazin și echipele mici** care facturează comenzile eMAG în **Oblio** și vor facturi și AWB-uri pregătite direct din comandă.
- **Agențiile și integratorii** care administrează conturi de vânzător și au nevoie de o evidență a fiecărei schimbări făcute de un asistent AI.

Ai nevoie de un cont Vendwiser (există un plan gratuit, vezi [vendwiser.ai](https://vendwiser.ai) pentru planuri) și de un cont de vânzător eMAG Marketplace România sau Trendyol cu acces API. Fiecare magazin Trendyol este un cont de marketplace separat: planurile Gratuit, Starter și Pro includ un cont per marketplace, Scale până la șase, adică toate țările Trendyol. Oblio este opțional.

## Funcții

- **Catalog:** oferte cu preț, stoc, stare, stare de validare și poziție la butonul de cumpărare; numărători exacte ale catalogului; mesaje de validare complete; regulile categoriilor; cote de TVA și timpi de procesare (eMAG).
- **Pagini de produs (Trendyol):** citești pagina unui produs și propui schimbări de titlu, descriere (întreagă sau înlocuire de fragmente), atribute și zile de livrare; propui produse noi; ștergi variante arhivate.
- **Prețuri și stoc:** propui prețuri noi, stocuri noi și activarea sau dezactivarea ofertelor, mereu întâi ca previzualizare.
- **Comenzi:** listezi, numeri și deschizi comenzi; găsești comenzile care au depășit termenul de predare; propui confirmarea comenzilor noi; pe Trendyol împarți o comandă în colete sau anulezi produsele care nu pot fi livrate.
- **Livrare (AWB, doar eMAG):** propui un AWB pentru o comandă prin conturile de curier eMAG, cu greutatea și coletele sugerate din datele produsului; citești AWB-urile emise prin Vendwiser.
- **Retururi:** cererile de retur, cele mai noi primele; pe Trendyol propui acceptarea sau refuzul unui retur.
- **Întrebările clienților (Trendyol):** vezi întrebările de pe paginile produselor și propui un răspuns; Trendyol îl verifică înainte să-l publice.
- **Finanțe (Trendyol):** vânzări, retururi, reduceri, cupoane, plăți și facturile de comision și transport reținute, exact cum le raportează Trendyol.
- **Campanii (doar eMAG):** campaniile la care te poți înscrie, cu date și reguli; propui produse pentru o campanie.
- **Facturare (Oblio):** listezi și deschizi facturi și proforme; propui emiterea unei facturi, emiterea facturii pentru o comandă de marketplace în lei (eMAG România, Trendyol România; cumpărătorul, produsele, transportul și voucherele se iau din comandă), sau anularea unei facturi.
- **Analize (eMAG și Trendyol):** auditul sănătății catalogului, verificarea butonului de cumpărare cu diferența față de prețul câștigător, paritatea prețurilor între oricare două conturi (două țări Trendyol sau eMAG și Trendyol; fără conversie valutară și fără ajustare de TVA), raportul comenzilor neexpediate.
- **Siguranță:** propuneri cu aprobare în conversație, reguli de protecție și jurnal de activitate exportabil (vezi mai jos).

## Modelul de siguranță

Vendwiser e construit astfel încât un asistent AI **să poată citi liber, dar să nu-ți poată schimba niciodată magazinul singur**.

1. **Citirea e imediată.** Ofertele, paginile de produs, stocul, prețurile, comenzile, retururile, întrebările clienților, finanțele, campaniile, facturile și rapoartele vin direct din conturile tale.
2. **Orice scriere este o propunere.** Uneltele care schimbă ceva (prețuri, stoc, starea ofertelor, pagini de produs, produse noi sau șterse, confirmarea comenzilor, împărțiri și anulări în comenzi, decizii de retur, răspunsuri la întrebările clienților, facturi, AWB-uri, înscrieri în campanii) doar *simulează* schimbarea. Întorc o previzualizare înainte și după și un identificator de acțiune, de exemplu `act_1a2b3c`. În acest moment nimic nu ajunge la eMAG, Trendyol sau Oblio.
3. **Aprobi în conversație.** Asistentul îți arată fraza exactă de aprobare, de exemplu:

   ```text
   I approve act_1a2b3c: price of NW-MAT-180 to 1415
   ```

   Doar după ce răspunzi cu ea, asistentul apelează `manage_action` ca să aplice acea singură schimbare. O poți și respinge. Aprobarea se înregistrează împreună cu numele clientului AI care a aplicat-o.
4. **Regulile de protecție blochează schimbările riscante înainte să fie puse în așteptare**: o scădere maximă de preț per schimbare (de exemplu 15%), un preț minim și o limită a numărului de oferte pe care o singură schimbare le poate atinge. O schimbare blocată este refuzată cu numele regulii; nu se pune nimic în așteptare.
5. **Totul se înregistrează.** Fiecare apel de unealtă și fiecare aprobare intră într-un jurnal de activitate pe care îl citești cu `get_activity` și îl exporți din panoul de control.

Alte protecții: datele de acces la marketplace și la facturare sunt stocate criptat și nu sunt arătate niciodată asistentului; telefoanele, emailurile și adresele cumpărătorilor nu sunt transmise asistentului (comenzile arată doar numele și orașul cumpărătorului). `manage_action` este singura unealtă marcată `destructiveHint: true`, iar toate uneltele au `openWorldHint: false`. Unele schimbări aprobate nu pot fi anulate pe marketplace (ștergerea variantelor Trendyol, anularea produselor dintr-o comandă); previzualizarea o spune clar. Detalii în [docs/security.md](docs/security.md) (în engleză).

## Conectare rapidă

Adresa serverului este aceeași peste tot: **`https://app.vendwiser.ai/mcp`**. La prima folosire, clientul îți deschide o fereastră de browser în care te autentifici în Vendwiser și apeși **Allow access**.

### Claude (claude.ai și Claude Desktop)

**Cu un clic:** [**Adaugă Vendwiser în Claude**](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Vendwiser&connectorUrl=https%3A%2F%2Fapp.vendwiser.ai%2Fmcp)

Sau manual: **Settings → Connectors → Add custom connector**, nume `Vendwiser`, adresa `https://app.vendwiser.ai/mcp`, apoi **Connect**. Pe planurile Team și Enterprise poate fi nevoie ca un proprietar al organizației să adauge întâi conectorul.

### Claude Code

```bash
claude mcp add --transport http vendwiser https://app.vendwiser.ai/mcp
```

Apoi rulezi `/mcp` în Claude Code și alegi **Authenticate** pentru `vendwiser`.

### ChatGPT

1. Mergi la **Settings → Apps → Advanced settings** și pornești **Developer mode** (disponibil pe planurile care permit aplicații MCP personalizate).
2. Alegi **Create app**, nume `Vendwiser`, adresa `https://app.vendwiser.ai/mcp` și autentificarea **OAuth**.
3. Te autentifici în Vendwiser în fereastra care se deschide și apeși **Allow access**.

Dacă Vendwiser apare în directorul de aplicații ChatGPT din regiunea ta, îl poți adăuga direct de acolo.

### Cursor

**Cu un clic:** [**Add to Cursor**](https://cursor.com/en/install-mcp?name=vendwiser&config=eyJ1cmwiOiJodHRwczovL2FwcC52ZW5kd2lzZXIuYWkvbWNwIn0%3D)

Sau adaugi în `~/.cursor/mcp.json` (global) ori `.cursor/mcp.json` (proiect):

```json
{
  "mcpServers": {
    "vendwiser": {
      "url": "https://app.vendwiser.ai/mcp"
    }
  }
}
```

Depozitul este și un plugin Cursor (`.cursor-plugin/plugin.json`).

### VS Code (GitHub Copilot)

**Cu un clic:** [**Install in VS Code**](https://insiders.vscode.dev/redirect/mcp/install?name=vendwiser&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapp.vendwiser.ai%2Fmcp%22%7D)

Sau din terminal:

```bash
code --add-mcp '{"name":"vendwiser","type":"http","url":"https://app.vendwiser.ai/mcp"}'
```

Sau rulezi **MCP: Add Server** din Command Palette, alegi **HTTP** și introduci adresa. Intrarea echivalentă din `mcp.json`:

```json
{
  "servers": {
    "vendwiser": {
      "type": "http",
      "url": "https://app.vendwiser.ai/mcp"
    }
  }
}
```

### Gemini CLI

Instalezi extensia din acest depozit:

```bash
gemini extensions install https://github.com/cadarsergiu/vendwiser-mcp
```

Sau adaugi doar serverul:

```bash
gemini mcp add --transport http vendwiser https://app.vendwiser.ai/mcp
```

Pornești `gemini` și rulezi `/mcp auth vendwiser` ca să te autentifici.

### Cline

Deschizi **MCP Servers → Remote Servers**, nume `vendwiser`, adresa `https://app.vendwiser.ai/mcp`, și finalizezi autentificarea. Sau adaugi în `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "vendwiser": {
      "type": "streamableHttp",
      "url": "https://app.vendwiser.ai/mcp"
    }
  }
}
```

Instrucțiuni de instalare pentru agenți: [llms-install.md](llms-install.md).

### Windsurf

Adaugi în `~/.codeium/windsurf/mcp_config.json`, reîmprospătezi panoul MCP și te autentifici:

```json
{
  "mcpServers": {
    "vendwiser": {
      "serverUrl": "https://app.vendwiser.ai/mcp"
    }
  }
}
```

### Orice alt client MCP

| Setare | Valoare |
|---|---|
| Adresă | `https://app.vendwiser.ai/mcp` |
| Transport | Streamable HTTP (`streamable-http`), fără stare |
| Autentificare | OAuth 2.1, înregistrare dinamică a clientului, PKCE `S256`, client public (fără secret) |
| Metadate resursă protejată | `https://app.vendwiser.ai/.well-known/oauth-protected-resource` |
| Metadate server de autorizare | `https://app.vendwiser.ai/.well-known/oauth-authorization-server` |
| Scope | `vendwiser` |

O cerere neautentificată primește `401` cu antetul `WWW-Authenticate: Bearer resource_metadata="…"`, așa că un client conform specificației descoperă singur fluxul OAuth. O configurație gata făcută este în [mcp.json](mcp.json).

Pentru clienții care acceptă doar servere locale (stdio), puntea comunitară [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) poate face legătura cu serverul la distanță:

```json
{
  "mcpServers": {
    "vendwiser": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://app.vendwiser.ai/mcp"]
    }
  }
}
```

## Unelte

Ultima coloană arată ce marketplace-uri acceptă fiecare unealtă (în clientul tău, rândul „Works on:” al fiecărei unelte arată doar conturile pe care le-ai conectat): **eMAG** (România), **Trendyol** (România, Bulgaria, Grecia, Cehia, Slovacia, Germania) sau **Oblio**. Legendă: 📖 **Citire** (`readOnlyHint: true`, întoarce date) · ✍️ **Propunere** (simulează o schimbare și așteaptă aprobarea ta) · 🔀 **Router** (rulează altă unealtă după nume și se comportă exact ca ea) · ⚠️ **Distructivă** (aplică sau respinge o schimbare pe care ai aprobat-o; `destructiveHint: true`). Toate uneltele au `openWorldHint: false`.

### Pornire și găsirea uneltelor

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`start_here`](docs/tools.md#start_here) | Ce poate face Vendwiser acum: conturi conectate, apeluri rămase, schimbări în așteptare. | 📖 Citire | Spațiu de lucru |
| [`search_tools`](docs/tools.md#search_tools) | Găsește unealta potrivită pentru o sarcină descrisă în cuvinte simple. | 📖 Citire | Spațiu de lucru |
| [`get_tool_schema`](docs/tools.md#get_tool_schema) | Parametrii exacți ai uneia sau mai multor unelte. | 📖 Citire | Spațiu de lucru |
| [`run_tool`](docs/tools.md#run_tool) | Rulează orice unealtă după nume (se comportă exact ca unealta apelată). | 🔀 Router | Spațiu de lucru |

### Conturi și consum

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`list_accounts`](docs/tools.md#list_accounts) | Conturile de marketplace conectate și ce are voie asistentul pe fiecare. | 📖 Citire | Spațiu de lucru |
| [`connection_health`](docs/tools.md#connection_health) | Verifică dacă fiecare cont conectat răspunde. | 📖 Citire | Spațiu de lucru |
| [`get_usage_status`](docs/tools.md#get_usage_status) | Apeluri folosite și incluse luna aceasta, cost apeluri extra. | 📖 Citire | Spațiu de lucru |

### Aprobări și jurnal de activitate

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`list_pending_actions`](docs/tools.md#list_pending_actions) | Schimbările simulate care așteaptă aprobare. | 📖 Citire | Spațiu de lucru |
| [`get_action`](docs/tools.md#get_action) | Starea și rezultatul unei schimbări propuse. | 📖 Citire | Spațiu de lucru |
| [`manage_action`](docs/tools.md#manage_action) | Aplică sau respinge o schimbare, doar după aprobarea explicită a vânzătorului. | ⚠️ Distructivă | Spațiu de lucru |
| [`get_activity`](docs/tools.md#get_activity) | Ultimele intrări din jurnalul de activitate. | 📖 Citire | Spațiu de lucru |

### Catalog și oferte

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`offers_list`](docs/tools.md#offers_list) | Oferte cu preț, stoc, stare, validare și poziție la butonul de cumpărare. | 📖 Citire | eMAG, Trendyol |
| [`offers_summary`](docs/tools.md#offers_summary) | Numărători exacte: total, active, inactive, respinse. | 📖 Citire | eMAG, Trendyol |
| [`offers_get`](docs/tools.md#offers_get) | Detaliile complete ale unei oferte, cu toate mesajele de validare. | 📖 Citire | eMAG, Trendyol |
| [`offers_validation_errors`](docs/tools.md#offers_validation_errors) | Ofertele respinse sau blocate, cu motivele date de marketplace. | 📖 Citire | eMAG, Trendyol |
| [`categories_get`](docs/tools.md#categories_get) | Regulile unei categorii: caracteristici obligatorii, valori permise, EAN. | 📖 Citire | eMAG, Trendyol |
| [`vat_list`](docs/tools.md#vat_list) | Cotele de TVA disponibile pe cont. | 📖 Citire | eMAG |
| [`handling_time_list`](docs/tools.md#handling_time_list) | Valorile de timp de procesare acceptate. | 📖 Citire | eMAG |
| [`offers_update_price`](docs/tools.md#offers_update_price) | Propune prețuri noi (fără TVA pe eMAG, cu TVA pe Trendyol). | ✍️ Propunere | eMAG, Trendyol |
| [`offers_update_stock`](docs/tools.md#offers_update_stock) | Propune valori noi de stoc. | ✍️ Propunere | eMAG, Trendyol |
| [`offers_update_status`](docs/tools.md#offers_update_status) | Propune activarea sau dezactivarea ofertelor. | ✍️ Propunere | eMAG, Trendyol |
| [`offers_content_get`](docs/tools.md#offers_content_get) | Conținutul paginii unui produs: titlu, descriere, imagini, atribute, atribute necompletate, zile de livrare. | 📖 Citire | Trendyol |
| [`offers_update_content`](docs/tools.md#offers_update_content) | Propune schimbări de titlu, descriere (întreagă sau înlocuire de fragmente), atribute și zile de livrare; Trendyol le verifică înainte să le afișeze. | ✍️ Propunere | Trendyol |
| [`offers_create`](docs/tools.md#offers_create) | Propune produse noi (variantele au același product_main_id); Trendyol verifică fiecare produs înainte de vânzare. | ✍️ Propunere | Trendyol |
| [`offers_delete`](docs/tools.md#offers_delete) | Propune ștergerea variantelor arhivate de peste o zi. Nu se poate anula. | ✍️ Propunere | Trendyol |

### Comenzi și livrare

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`orders_list`](docs/tools.md#orders_list) | Comenzi dintr-un interval (implicit ultimele 7 zile); cumpărătorul redus la nume și oraș. | 📖 Citire | eMAG, Trendyol |
| [`orders_get`](docs/tools.md#orders_get) | O comandă cu produsele, AWB-ul și starea facturii. | 📖 Citire | eMAG, Trendyol |
| [`orders_count`](docs/tools.md#orders_count) | Numărul de comenzi, opțional pe o stare. | 📖 Citire | eMAG, Trendyol |
| [`couriers_list`](docs/tools.md#couriers_list) | Conturile de curier configurate. | 📖 Citire | eMAG |
| [`awb_get`](docs/tools.md#awb_get) | Un AWB emis prin Vendwiser. | 📖 Citire | eMAG |
| [`orders_acknowledge`](docs/tools.md#orders_acknowledge) | Propune confirmarea comenzilor noi. | ✍️ Propunere | eMAG, Trendyol |
| [`orders_attach_invoice`](docs/tools.md#orders_attach_invoice) | Propune atașarea unei facturi PDF la o comandă. | ✍️ Propunere | eMAG, Trendyol |
| [`orders_issue_awb`](docs/tools.md#orders_issue_awb) | Propune un AWB pentru o comandă. Doar eMAG. | ✍️ Propunere | eMAG |
| [`orders_split`](docs/tools.md#orders_split) | Propune trimiterea unei părți din comandă în colete separate. | ✍️ Propunere | Trendyol |
| [`orders_cancel_items`](docs/tools.md#orders_cancel_items) | Propune anularea produselor dintr-o comandă care nu pot fi livrate. Nu se poate anula și contează împotriva magazinului. | ✍️ Propunere | Trendyol |

### Retururi

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`returns_list`](docs/tools.md#returns_list) | Cererile de retur, cele mai noi primele. | 📖 Citire | eMAG, Trendyol |
| [`returns_decide`](docs/tools.md#returns_decide) | Propune acceptarea (rambursează clientul) sau refuzul unui retur, cu unul dintre motivele marketplace-ului. | ✍️ Propunere | Trendyol |

### Întrebările clienților (Trendyol)

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`questions_list`](docs/tools.md#questions_list) | Întrebările clienților de pe paginile produselor, cele mai noi primele, și dacă mai așteaptă răspuns. | 📖 Citire | Trendyol |
| [`questions_answer`](docs/tools.md#questions_answer) | Propune un răspuns public; se trimite doar după aprobare, iar Trendyol îl verifică înainte să-l afișeze. | ✍️ Propunere | Trendyol |

### Finanțe (Trendyol)

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`finance_transactions`](docs/tools.md#finance_transactions) | Vânzări, retururi, reduceri, cupoane, plăți și facturile de comision și transport reținute, cum le înregistrează Trendyol (maximum 15 zile pe apel). | 📖 Citire | Trendyol |

### Campanii (eMAG)

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`campaigns_list`](docs/tools.md#campaigns_list) | Campaniile la care se poate înscrie vânzătorul, cu date și reguli. | 📖 Citire | eMAG |
| [`campaigns_propose`](docs/tools.md#campaigns_propose) | Propune produse pentru o campanie. | ✍️ Propunere | eMAG |

### Facturare (Oblio)

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`invoice_series_list`](docs/tools.md#invoice_series_list) | Seriile de documente și cotele de TVA din contul de facturare. | 📖 Citire | Oblio |
| [`invoices_list`](docs/tools.md#invoices_list) | Facturi sau proforme, cu client, total, stare și link PDF. | 📖 Citire | Oblio |
| [`invoices_get`](docs/tools.md#invoices_get) | O factură sau proformă după serie și număr. | 📖 Citire | Oblio |
| [`invoices_issue`](docs/tools.md#invoices_issue) | Propune emiterea unei facturi sau proforme. | ✍️ Propunere | Oblio |
| [`invoices_issue_for_order`](docs/tools.md#invoices_issue_for_order) | Propune factura pentru o comandă de marketplace în lei (eMAG România, Trendyol România). | ✍️ Propunere | Oblio |
| [`invoices_cancel`](docs/tools.md#invoices_cancel) | Propune anularea unei facturi sau proforme. | ✍️ Propunere | Oblio |

### Analize și rapoarte

| Unealtă | Ce face | Tip | Acceptă |
|---|---|---|---|
| [`catalog_health_audit`](docs/tools.md#catalog_health_audit) | Sănătatea catalogului: respinse, fără stoc, fără EAN, fără buton de cumpărare. | 📖 Citire | eMAG, Trendyol |
| [`buy_button_check`](docs/tools.md#buy_button_check) | Ofertele active care nu dețin butonul de cumpărare, cu diferența de preț. | 📖 Citire | eMAG, Trendyol |
| [`price_parity_check`](docs/tools.md#price_parity_check) | Compară prețurile acelorași SKU-uri între două conturi (de ex. două țări Trendyol sau eMAG și Trendyol), fără conversie valutară sau ajustare de TVA. | 📖 Citire | eMAG, Trendyol |
| [`unshipped_orders_report`](docs/tools.md#unshipped_orders_report) | Comenzi neexpediate mai vechi de N ore, cu întârzierea față de termenul de predare. | 📖 Citire | eMAG, Trendyol |

Parametrii și descrierile complete ale fiecărei unelte (în engleză): [docs/tools.md](docs/tools.md).

## Exemple de întrebări

Asistentul înțelege și româna, deci poți întreba direct:

- *Ce comenzi deschise au depășit termenul de predare?*
- *Câte oferte am și câte sunt respinse?*
- *Ce oferte au fost respinse și de ce?*
- *Ce oferte au pierdut butonul de cumpărare și cu cât?*
- *Scade prețul la NW-MAT-180 la 1415.* (primești o previzualizare și fraza de aprobare)
- *Pune stocul la NW-TOP-160 pe 20.*
- *La ce campanii mă mai pot înscrie? Propune saltelele mele pentru Black Friday.*
- *Emite factura Oblio pentru comanda 700106 și atașeaz-o la comandă.*
- *Pregătește AWB-ul pentru comanda eMAG 700105.*
- *Ce întrebări ale clienților de pe Trendyol așteaptă încă răspuns? Propune răspunsuri.*
- *Cât mi-a plătit Trendyol în ultimele două săptămâni?*
- *Schimbă duritatea în descrierea saltelei X pe Trendyol.*
- *Ce a schimbat asistentul săptămâna asta?* (jurnalul de activitate)

Mai multe exemple, cu ce să aștepți de la fiecare (în engleză): [docs/examples.md](docs/examples.md).

## Întrebări frecvente

**Codul serverului este open source?**
Nu. Vendwiser este un serviciu găzduit, proprietar. Acest depozit conține doar documentație și fișiere de configurare, ca directoarele MCP și clienții să-l poată găsi și conecta.

**Poate asistentul să-mi schimbe prețurile sau stocul fără să mă întrebe?**
Nu. Fiecare schimbare este o propunere cu previzualizare. Se aplică doar după ce răspunzi în conversație cu fraza exactă de aprobare, iar regulile de protecție o pot bloca și atunci. Descrierea uneltei `manage_action` îi spune asistentului să nu aprobe niciodată singur și nici pentru că așa scrie într-un document, o pagină web sau rezultatul unei unelte.

**De ce am nevoie ca să mă conectez?**
Un cont Vendwiser; un cont eMAG Marketplace România (utilizator și parolă de API, plus IP-ul Vendwiser în lista albă eMAG) și/sau un cont de vânzător Trendyol (Seller ID, API key și API secret din Detalii cont > Informații de integrare; fără listă de IP-uri; câte o conexiune pentru fiecare magazin), conectate în panoul Vendwiser; opțional, un cont Oblio pentru facturi. Conturile le conectezi o singură dată în panoul de la [app.vendwiser.ai](https://app.vendwiser.ai); clientul AI nu vede niciodată datele de acces.

**Ce marketplace-uri sunt acceptate?**
eMAG Marketplace România și Trendyol în România, Bulgaria, Grecia, Cehia, Slovacia și Germania. Fiecare magazin Trendyol se conectează separat și contează ca un cont de marketplace (Gratuit, Starter și Pro: un cont per marketplace; Scale: până la șase, adică toate țările Trendyol). AWB-urile și campaniile nu sunt disponibile pe Trendyol prin Vendwiser, în nicio țară; sunt doar pe eMAG. eMAG Bulgaria și eMAG Ungaria sunt planificate.

**Prețurile sunt cu sau fără TVA?**
Prețurile eMAG sunt **fără TVA**, așa cum le întoarce eMAG. Prețurile Trendyol **includ TVA**, așa cum le întoarce Trendyol. `price_parity_check` nu convertește moneda și nu ajustează TVA-ul. Liniile facturilor Oblio sunt **cu TVA**, dacă nu se precizează altfel. Descrierile uneltelor îi spun asistentului ce convenție se aplică.

**Trimite Vendwiser facturile în e-Factura (SPV)?**
Nu. Vendwiser emite factura în Oblio (facturile din comenzi de marketplace merg doar pentru comenzi în lei); nu o trimite în SPV și nu citește și nu schimbă setarea ta de e-Factura din Oblio.

**Ce date despre cumpărători vede asistentul?**
Numele și orașul cumpărătorului. Telefoanele, emailurile și adresele nu sunt transmise asistentului.

**Câte apeluri pot face?**
Depinde de plan. Întreabă asistentul *„Câte apeluri mai am?”* (`get_usage_status`) sau vezi [vendwiser.ai](https://vendwiser.ai).

**Este Vendwiser afiliat cu eMAG, Trendyol sau Oblio?**
Nu. Vendwiser este independent și folosește API-urile publice eMAG Marketplace, Trendyol și Oblio, cu conturile pe care le conectezi tu.

**Cum mă deconectez?**
Ștergi conectorul din clientul AI, apoi revoci accesul sau deconectezi conturile de marketplace din panoul Vendwiser.

## Linkuri

- Site: <https://vendwiser.ai>
- Documentație: <https://vendwiser.ai/docs/>
- Trendyol în ChatGPT și Claude: <https://vendwiser.ai/trendyol-chatgpt/> ([English](https://vendwiser.ai/en/trendyol-chatgpt/))
- eMAG în ChatGPT: <https://vendwiser.ai/emag-chatgpt/> · eMAG în Claude: <https://vendwiser.ai/emag-claude/>
- Oblio în ChatGPT și Claude: <https://vendwiser.ai/oblio-chatgpt/> · Factură Oblio din comanda eMAG: <https://vendwiser.ai/facturi-emag-oblio/>
- Politica de confidențialitate: <https://vendwiser.ai/privacy/>
- Termeni și condiții: <https://vendwiser.ai/terms/>
- Acordul de prelucrare a datelor: <https://vendwiser.ai/dpa/>
- Suport: [hello@vendwiser.ai](mailto:hello@vendwiser.ai)

## Licență

Documentația din acest depozit este licențiată sub [CC BY 4.0](LICENSES/CC-BY-4.0.txt), iar fișierele de configurare sub [licența MIT](LICENSE). Serverul, serviciul și marca Vendwiser sunt proprietare și **nu** sunt acoperite de aceste licențe. Vezi [LICENSE](LICENSE).

eMAG, Trendyol și Oblio sunt mărci ale proprietarilor lor și sunt folosite aici doar pentru a descrie compatibilitatea.

---

© 2026 VOLTIVO POWER TRADING SRL, Cluj-Napoca, România.
