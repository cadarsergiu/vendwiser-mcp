# Cum publici depozitul și unde îl trimiți

Ghid pas cu pas pentru Sergiu. Nimic din acest depozit nu a fost publicat încă; toți pașii de mai jos îi faci tu, din conturile tale.

> **Regulă de aur:** în depozit nu intră nicio parolă, niciun token, cheie privată sau date reale de clienți. Parola contului de recenzent o treci doar în formularele private ale directoarelor care o cer, niciodată în fișiere.

## 0. Ce îți trebuie

- Un **cont GitHub personal** (nu o organizație). LobeHub a respins mult timp depozitele deținute de organizații (problema [#17416](https://github.com/lobehub/lobehub/issues/17416)); cu un cont personal ești sigur.
- `git` instalat (sau GitHub Desktop).
- Pentru registrul MCP oficial: acces la DNS-ul domeniului `vendwiser.ai` **sau** posibilitatea de a pune un fișier pe `https://vendwiser.ai/.well-known/`.
- Pentru LobeHub: Node.js 22 sau mai nou.

## 1. Pregătește fișierele

1. Dezarhivează `vendwiser-mcp-repo.zip`. Folderul conține și foldere ascunse (`.cursor-plugin`, `.github`) și `.gitignore`; pe macOS le vezi în Finder cu `Cmd+Shift+.`.
2. Înlocuiește peste tot `cadarsergiu` cu numele tău de utilizator GitHub (apare în `README.md`, `README.ro.md`, `.cursor-plugin/plugin.json` și `.github/ISSUE_TEMPLATE/config.yml`). În terminal, din folderul depozitului:

   ```bash
   # Linux
   grep -rl --exclude=PUBLISHING.md cadarsergiu . | xargs sed -i 's/cadarsergiu/numele-tau/g'
   # macOS
   grep -rl --exclude=PUBLISHING.md cadarsergiu . | xargs sed -i '' 's/cadarsergiu/numele-tau/g'
   ```

3. Citește o dată `README.md` și `README.ro.md`. Comentariul HTML de sub „Any other MCP client” din README-ul englez spune că serverul acceptă și o cheie API ca Bearer token; dacă vrei să documentezi asta, completează acolo de unde se creează cheia, altfel șterge comentariul.

## 2. Creează depozitul pe GitHub

1. Intră pe <https://github.com/new>.
2. **Owner:** contul tău personal. **Repository name:** `vendwiser-mcp`. **Public.** Nu bifa „Add a README”, „.gitignore” sau „license” (le ai deja).
3. Încarcă fișierele cu git (încărcarea din browser poate pierde folderele ascunse):

   ```bash
   cd vendwiser-mcp-repo
   git init -b main
   git add .
   git commit -m "Vendwiser MCP docs 1.0.0"
   git remote add origin https://github.com/numele-tau/vendwiser-mcp.git
   git push -u origin main
   ```

4. Creează eticheta și release-ul (Gemini CLI le cere):

   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

   Apoi pe GitHub: **Releases → Draft a new release**, alegi eticheta `v1.0.0`, titlul `1.0.0`, copiezi textul din `CHANGELOG.md` și apeși **Publish release**.

5. În pagina depozitului, la **About** (rotița din dreapta):
   - **Description:** `Docs and client configs for Vendwiser, the remote MCP server for eMAG Marketplace Romania and Oblio invoicing.`
   - **Website:** `https://vendwiser.ai`
   - **Topics:** `mcp`, `mcp-server`, `model-context-protocol`, `remote-mcp`, `emag`, `oblio`, `ecommerce`, `marketplace`, `romania`, `gemini-cli-extension`, `cursor-plugin`

## 3. Unde trimiți, în ordine

| # | Director | Ce faci | Ce îți trebuie |
|---|---|---|---|
| 1 | **Registrul MCP oficial** | Pașii din secțiunea 4. Te listează automat și în GitHub MCP Registry / VS Code și în PulseMCP. | DNS sau fișier pe vendwiser.ai |
| 2 | **Smithery** | <https://smithery.ai/new> → introduci `https://app.vendwiser.ai/mcp` → finalizezi → **Settings → Verification**. Ca să vadă uneltele fără autentificare, publică întâi `hosting/.well-known/mcp/server-card.json` la `https://app.vendwiser.ai/.well-known/mcp/server-card.json`. Nu e nevoie de `smithery.yaml` (acela e doar pentru servere găzduite de Smithery). | cont Smithery |
| 3 | **Glama – Connectors** | <https://glama.ai/mcp/connectors> → **Add Connector**. Apoi copiezi insigna și o decomentezi în ambele README-uri. | cont Glama |
| 4 | **punkpeye/awesome-remote-mcp-servers** | Dai **Star** depozitului (obligatoriu), apoi PR în secțiunea 🛒 E-Commerce, după CONTRIBUTING.md, cu insigna Glama. | insigna Glama |
| 5 | **mcpservers.org** | <https://mcpservers.org/submit>: nume, categorie, descriere scurtă, `https://vendwiser.ai/docs/`, bifezi „remote”, numele din registru `ai.vendwiser/vendwiser`. | email |
| 6 | **Cursor Directory** | <https://cursor.directory/plugins/new>, modul **Manual**, cu `{"mcpServers":{"vendwiser":{"url":"https://app.vendwiser.ai/mcp"}}}`. | cont GitHub/Google |
| 7 | **Cursor Marketplace** | <https://cursor.com/marketplace/publish> → linkul depozitului. Depozitul are deja `.cursor-plugin/plugin.json`, `mcp.json`, `rules/` și logo. | depozitul public |
| 8 | **MCP.so** | <https://mcp.so/submit> → URL-ul depozitului. | depozitul public |
| 9 | **MCP Market** | <https://mcpmarket.com/submit> → URL-ul depozitului. | depozitul public |
| 10 | **Galeria Gemini CLI** | Nimic de trimis: are nevoie doar de topicul `gemini-cli-extension` și de release-ul `v1.0.0` (pasul 2). Apare în <https://geminicli.com/extensions/> după crawl-ul zilnic. Test local: `gemini extensions install https://github.com/numele-tau/vendwiser-mcp`. | topic + release |
| 11 | **LobeHub** | Comenzile de mai jos. | Node 22, cont LobeHub |
| 12 | **Cline MCP Marketplace** | Issue nou cu șablonul [mcp-server-submission](https://github.com/cline/mcp-marketplace/issues/new?template=mcp-server-submission.yml): URL depozit, logo `assets/logo-400.png` (400×400), motivul. Confirmă că ai testat instalarea din `llms-install.md`. | depozitul public |
| 13 | **mcp.directory**, **jaw9c/awesome-remote-mcp-servers**, **goose** | Opționale: <https://mcp.directory/submit>; PR în jaw9c cu auth `OAuth2.1`; PR în goose cu tip `streamable-http`. | depozitul public |

### LobeHub (pasul 11)

`lhm.plugin.json` este deja completat (identificator `vendwiser`, categorie `business`, cele 52 de unelte, `cloudEndpoint`). Nu rula `plugin init` peste el (l-ar suprascrie doar cu `--force`).

```bash
cd vendwiser-mcp-repo
npx -y @lobehub/market-cli@latest login
npx -y @lobehub/market-cli@latest github connect
npx -y @lobehub/market-cli@latest plugin publish https://github.com/numele-tau/vendwiser-mcp --dir .
```

Dacă identificatorul `vendwiser` e deja luat, schimbă `identifier` în `lhm.plugin.json` (de exemplu `vendwiser-mcp`) și rulează din nou. Pentru versiuni noi: `npx -y @lobehub/market-cli@latest plugin update --dir .`

## 4. Registrul MCP oficial (`ai.vendwiser/vendwiser`)

Numele `ai.vendwiser/...` cere dovada că deții domeniul `vendwiser.ai`. Alegi **una** dintre metode: DNS (recomandat) sau fișier HTTP. Comenzile cer **OpenSSL 3**; pe macOS instalează-l cu `brew install openssl@3` și folosește `/opt/homebrew/opt/openssl@3/bin/openssl` (Apple Silicon) sau `/usr/local/opt/openssl@3/bin/openssl` (Intel) în loc de `openssl`.

### 4.1 Instalează `mcp-publisher`

```bash
# macOS / Linux, cu Homebrew
brew install mcp-publisher

# sau binarul oficial
curl -L "https://github.com/modelcontextprotocol/registry/releases/latest/download/mcp-publisher_$(uname -s | tr '[:upper:]' '[:lower:]')_$(uname -m | sed 's/x86_64/amd64/;s/aarch64/arm64/').tar.gz" | tar xz mcp-publisher && sudo mv mcp-publisher /usr/local/bin/

mcp-publisher --help
```

### 4.2 Generează perechea de chei (o singură dată)

Lucrează **în afara** folderului depozitului, ca cheia privată să nu ajungă din greșeală pe GitHub (`.gitignore` exclude oricum `*.pem`).

```bash
mkdir -p ~/vendwiser-registry && cd ~/vendwiser-registry
openssl genpkey -algorithm Ed25519 -out key.pem
PUBLIC_KEY="$(openssl pkey -in key.pem -pubout -outform DER | tail -c 32 | base64)"
echo "v=MCPv1; k=ed25519; p=${PUBLIC_KEY}"
```

Ultima comandă afișează un singur rând de forma `v=MCPv1; k=ed25519; p=AbC…=` (cheia publică, 44 de caractere după `p=`). Acesta e conținutul pe care îl publici. **`key.pem` este cheia privată**: păstreaz-o într-un manager de parole sau seif, nu o trimite nimănui și nu o pune pe server.

### 4.3a Varianta DNS (recomandată)

1. În panoul DNS al domeniului `vendwiser.ai` adaugi o înregistrare **TXT pe domeniul rădăcină** (gazda `@`, nu `_mcp` sau alt subdomeniu):
   - **Tip:** `TXT`
   - **Nume/Gazdă:** `@` (adică `vendwiser.ai`)
   - **Valoare:** rândul afișat mai sus, de exemplu `v=MCPv1; k=ed25519; p=AbC…=`
   - **TTL:** implicit

   Echivalentul în format de zonă: `vendwiser.ai. IN TXT "v=MCPv1; k=ed25519; p=AbC…="`. Înregistrările TXT existente (SPF, verificări Google etc.) rămân neatinse.

2. Verifici propagarea: `dig +short TXT vendwiser.ai` trebuie să arate rândul `v=MCPv1…`.
3. Te autentifici:

   ```bash
   cd ~/vendwiser-registry
   PRIVATE_KEY="$(openssl pkey -in key.pem -noout -text | grep -A3 "priv:" | tail -n +2 | tr -d ' :\n')"
   mcp-publisher login dns --domain "vendwiser.ai" --private-key "${PRIVATE_KEY}"
   ```

### 4.3b Varianta fișier HTTP

1. Creezi fișierul:

   ```bash
   cd ~/vendwiser-registry
   PUBLIC_KEY="$(openssl pkey -in key.pem -pubout -outform DER | tail -c 32 | base64)"
   echo "v=MCPv1; k=ed25519; p=${PUBLIC_KEY}" > mcp-registry-auth
   ```

2. Îl publici exact la `https://vendwiser.ai/.well-known/mcp-registry-auth` (fără redirecționare către `www`, tip `text/plain`). Conține doar rândul `v=MCPv1; k=ed25519; p=…`.
3. Verifici: `curl https://vendwiser.ai/.well-known/mcp-registry-auth`.
4. Te autentifici:

   ```bash
   PRIVATE_KEY="$(openssl pkey -in key.pem -noout -text | grep -A3 "priv:" | tail -n +2 | tr -d ' :\n')"
   mcp-publisher login http --domain "vendwiser.ai" --private-key "${PRIVATE_KEY}"
   ```

### 4.4 Validează și publică

Din folderul depozitului (acolo e `server.json`):

```bash
cd vendwiser-mcp-repo
mcp-publisher validate
mcp-publisher publish
```

Verifici listarea:

```bash
curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.vendwiser/vendwiser"
```

De reținut:

- O adresă de server poate aparține unui singur nume în registru, deci publică `ai.vendwiser/vendwiser` cât mai repede.
- O versiune publicată nu se mai poate modifica. Pentru orice schimbare în `server.json` crești `version` (de exemplu `1.0.1`) și publici din nou.
- Câmpul opțional `repository` lipsește intenționat din `server.json`, pentru că depozitul conține doar documentație, nu codul serverului. Dacă vrei totuși link spre depozit, adaugă înainte de prima publicare:

  ```json
  "repository": { "url": "https://github.com/numele-tau/vendwiser-mcp", "source": "github" }
  ```

- Tokenul de sesiune al `mcp-publisher` se salvează local în fișiere `.mcpregistry_*`; nu le pune pe GitHub (sunt deja în `.gitignore`).

## 5. Actualizări ulterioare

Când se schimbă ceva (unelte noi, texte noi):

1. Crești versiunea în `server.json`, `gemini-extension.json`, `.cursor-plugin/plugin.json` și `lhm.plugin.json` și adaugi o intrare în `CHANGELOG.md`.
2. Dacă s-a schimbat lista de unelte, actualizezi `docs/tools.md`, tabelele din README-uri, `lhm.plugin.json` și `hosting/.well-known/mcp/server-card.json`.
3. `git commit`, `git tag vX.Y.Z`, `git push --tags`, apoi un release nou pe GitHub.
4. `mcp-publisher publish` (registrul oficial) și `npx -y @lobehub/market-cli@latest plugin update --dir .` (LobeHub).

## 6. Ce rămâne în afara acestui ghid

Emailurile către eMAG SPN (spn@emag.ro) și Oblio (contact@oblio.eu), postările în grupuri și directoarele generale de AI sunt în `vendwiser-listing-venues.md`. Fiecare trimitere și fiecare email pleacă doar după ce le aprobi tu.
