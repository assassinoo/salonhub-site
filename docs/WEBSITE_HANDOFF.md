# SalonHub Website — Project Handoff

**Ultima actualizare:** iunie 2026  
**Scop:** document central pentru continuarea website-ului SalonHub într-un chat Cursor/ChatGPT nou, fără pierderea contextului.

**Cum citești statusurile din acest fișier:**

| Etichetă | Semnificație |
|----------|--------------|
| **În repo** | Confirmat în fișierele versionate din acest repository |
| **Deployat/validat manual** | Confirmat manual sau documentat cross-repo; **nu** inferat doar din existența fișierelor |
| **Neimplementat / următor pas** | Nu există în repo sau nu este livrat ca produs |

> **Important:** conținutul HTML din repo **nu** garantează automat că este live pe `salonhub.ro`. Secțiunea 2 separă ce este în cod față de ce este deployat.

---

## 1. Repository și mediu

| Item | Detaliu |
|------|---------|
| **Website repo** | `D:\SalonHub\salonhub-site` |
| **Mobile repo (separat)** | `D:\SalonHub\salonhub-mobile` |
| **OS / shell** | Windows 11, **PowerShell only** pentru comenzi locale |
| **Tip site** | HTML + CSS static, servit prin **Cloudflare Worker** (Static Assets) |
| **Worker entry** | `worker.js` — proxy minimal către binding `ASSETS` |
| **Deploy tool** | Wrangler (`npm run deploy` → `wrangler deploy`) |
| **Build** | Fără bundler frontend; `npm ci` instalează doar Wrangler |

**Separare repo-uri:** aplicația mobilă Expo/Supabase și website-ul public sunt proiecte git distincte. Modificările din un repo **nu** se propagă automat în celălalt.

**Fișiere excluse la upload asset-uri** (`.assetsignore`): `.git/`, `node_modules/`, `wrangler.jsonc`, `worker.js`, `package.json`, `README.md`, `review_*.txt`, etc. — doar conținutul static public (HTML, CSS, subfoldere pagini) ajunge la edge.

---

## 2. Domeniu și deploy

### În repo

| Item | Sursă |
|------|--------|
| Nume Worker | `salonhub-site` (`wrangler.jsonc` → `"name": "salonhub-site"`) |
| Static assets root | directorul rădăcină al repo-ului |
| Script deploy | `package.json` → `"deploy": "wrangler deploy"` |
| Deploy din Git (documentat în README) | Build: `npm ci` · Deploy: `npm run deploy` |
| Verificare înainte de deploy | `npm.cmd run deploy -- --dry-run` |

**Nu sunt documentate aici:** Cloudflare account IDs, API tokens, project IDs workers.dev sau alte valori sensibile. `wrangler.jsonc` conține doar numele worker-ului, `main`, `compatibility_date` și binding assets — fără secrete.

### Deployat/validat manual

| Item | Status |
|------|--------|
| **Domeniu public** | https://salonhub.ro |
| **Worker Cloudflare** | `salonhub-site` |
| **Redirect www** | `www.salonhub.ro` → `salonhub.ro` |
| **Conexiune repo → deploy** | Flux Git Worker în Cloudflare Dashboard (conform README) |

> Starea live a DNS, domeniului custom și redirect-ului **www** este configurată în Cloudflare Dashboard, **nu** în fișierele HTML/CSS din repo. Confirmă întotdeauna cu output deploy sau verificare manuală înainte de a afirma că un change este live.

### Neimplementat / următor pas

- CI/CD separat de Cloudflare Git integration (doar fluxul documentat în README)
- Preview environments / staging URL dedicat pentru site
- Analytics, A/B testing, CMS

---

## 3. Pagini publice actuale

Toate paginile sunt **statice** (HTML + CSS). **Nu există** formulare cu backend, API propriu, newsletter, plăți sau autentificare pe site.

| URL | Fișier | Scop |
|-----|--------|------|
| `/` | `index.html` | Landing SalonHub: hero, cum funcționează, pentru saloane, despre. CTA „Aplicația este în pregătire” (buton dezactivat). Lansare inițială menționată: Târgu Mureș. |
| `/stergere-cont` | `stergere-cont/index.html` | Instrucțiuni ștergere cont din app (Cont → Ștergere profil) + alternativă **mailto:** către `stergere-cont@salonhub.ro`. Fără formular server-side. |
| `/confidentialitate` | `confidentialitate/index.html` | Politică de confidențialitate (ultima actualizare în pagină: 1 iulie 2026). Descrie categorii de date, Supabase, Cloudflare, drepturi utilizator. |
| `/termeni` | `termeni/index.html` | Termeni și condiții (ultima actualizare: 1 iulie 2026). Rol platformă vs servicii prestate de saloane. |

**Comportament comun:**

- Header cu logo textual link către `/`
- Footer cu linkuri legale către cele trei pagini + copyright © 2026 SalonHub
- Stiluri partajate: `/styles.css` (root) sau `styles.css` relativ pe landing
- Paginile legale folosesc clase `.page-content`, `.content-narrow`, `.content-block`

**Deployat/validat manual:** paginile de mai sus sunt documentate ca live pe https://salonhub.ro în handoff-ul mobil (`PROJECT_HANDOFF.md` §7). Verificarea din acest audit a fost **din conținut repo**, nu HTTP live în sesiunea curentă.

---

## 4. Legal / store readiness

### `/stergere-cont`

- Explică ștergerea **din aplicația mobilă** (Cont → Ștergere profil).
- Oferă alternativă externă prin **email** (`mailto:stergere-cont@salonhub.ro`) cu subiect/corp precompletat — utilizatorul trimite din clientul său de email; site-ul **nu** procesează cererea.
- Descrie pașii post-solicitare (verificare titular, blocaje posibile, ștergere/anonimizare).

### `/confidentialitate`

- Identifică operatorul ca **Marton Andrei, SalonHub** (secțiunea „Cine suntem”).
- Contact privacy: `contact@salonhub.ro` (link mailto).
- Menționează Supabase (app/backend) și Cloudflare (site public).
- Leagă ștergerea contului de `/stergere-cont`.

### `/termeni`

- Delimitează rolul SalonHub (platformă de descoperire/programări) față de **serviciile efective prestate de saloane**.
- Operator: **Marton Andrei, SalonHub**.
- Programări: salonul confirmă/gestionează; SalonHub nu este parte la relația comercială client–salon.

### Footer (toate paginile)

Linkuri către:

- Politica de confidențialitate → `/confidentialitate`
- Termeni și condiții → `/termeni`
- Ștergere cont → `/stergere-cont`

### Store baseline

| Aspect | Status |
|--------|--------|
| Pagini legale + ștergere cont pe web | **În repo**; documentate ca live manual |
| Play Store / App Store Data Safety | **Necompletat** |
| Store listing (descrieri, screenshot-uri) | **Necompletat** |
| Distribuție publică app | **Neimplementat** — vezi repo mobil |

Aceste pagini reprezintă **baza legală** pentru cerințele store, nu înlocuiesc complet formularele Data Safety sau listing-urile din console.

---

## 5. Email routing

**Configurație Cloudflare Email Routing** — **Deployat/validat manual** (setări în Cloudflare Dashboard, **nu** în repo website).

Adrese publice configurate:

| Adresă | Utilizare în repo |
|--------|-------------------|
| `contact@salonhub.ro` | Privacy, termeni, contact general |
| `suport@salonhub.ro` | Documentată ca rutată; **nu** apare în HTML-ul curent al site-ului |
| `stergere-cont@salonhub.ro` | Pagina `/stergere-cont` (mailto) |

**Forward:** toate sunt forwardate către un **inbox separat de business**. Adresa de destinație **nu** este documentată aici.

**Limitări (factual):**

- **Nu există** inbox nativ `@salonhub.ro` (primirea depinde de Email Routing + inbox destinație).
- **Nu există** trimitere outbound garantată ca `@salonhub.ro` fără SMTP/provider separat.
- Linkurile `mailto:` deschid clientul utilizatorului; site-ul **nu** trimite email.

**Regulă:** nu modifica Cloudflare Email Routing fără confirmare explicită.

---

## 6. Branding și assets

| Item | Status |
|------|--------|
| Logo site curent | **Textual** „SalonHub” (clasă `.logo` în header — font-weight 700, fără imagine) |
| Favicon | **Neimplementat** în repo (fără `<link rel="icon">` în HTML) |
| Imagini marketing | **Nu există** asset-uri PNG/SVG logo în repo website |
| Branding app / store | **Ne finalizat / neaprobat** — icon-uri template Expo sunt în **repo mobil** (`salonhub-mobile/assets/images`), nu aici |
| Imagini generate recent (mobil, untracked) | **Nu** sunt logo oficial; nu le documentăm ca branding aprobat |

**Următorul pas branding:** alegerea și exportarea unui simbol final care funcționează la dimensiuni mici (favicon, app icon, header site).

---

## 7. Reguli de lucru

1. **Nu modifica** DNS, Cloudflare Dashboard, Email Routing sau setări domeniu fără confirmare explicită.
2. **Deploy site:** verifică întâi cu `npm.cmd run deploy -- --dry-run`; nu afirma că un deploy este live fără output sau confirmare.
3. **Fără secrete** în repository (token-uri Wrangler, API keys, adrese forward email).
4. **PowerShell-safe:** `npm.cmd`, `npx.cmd`; evită presupuneri despre `npm` fără `.cmd` pe Windows.
5. **Review file UTF-8** obligatoriu în root pentru schimbări viitoare (`review_*.txt`).
6. **Nu face commit/push automat.**
7. **Separă repo vs live** — existența unui fișier HTML nu înseamnă că este deja publicat.
8. **Nu inventa** analytics, formulare backend, newsletter, plăți sau servicii inexistente.
9. Modificări HTML/CSS/Worker: pași mici; test local (`Start-Process .\index.html` sau dry-run deploy).

---

## 8. Următorii pași recomandați

1. **Finalizează brandingul / logo-ul oficial** (decizie cross-repo mobil + site).
2. **Pregătește favicon / logo website** după alegerea brandingului (export dimensiuni mici).
3. **Completează store readiness** în repo-ul mobil (`docs/PROJECT_HANDOFF.md` §8).
4. **Primul EAS development/preview build** (mobil).
5. **Testează push notifications** pe build real (nu Expo Go).
6. **TestFlight / Play Closed Testing.**
7. **Actualizează site-ul** doar dacă sunt necesare schimbări de store listing sau landing page (ex. link store, copy lansare).

---

## 9. Legătură cu repo-ul mobil

| Document | Locație |
|----------|---------|
| **Handoff mobil (canonic)** | `D:\SalonHub\salonhub-mobile\docs\PROJECT_HANDOFF.md` |
| **Handoff website (acest fișier)** | `D:\SalonHub\salonhub-site\docs\WEBSITE_HANDOFF.md` |

Pentru continuarea proiectului **SalonHub complet**, un chat nou trebuie să citească **ambele** handoff-uri:

- **Mobil** — app, Supabase, booking, account deletion, EAS, push.
- **Website** — pagini legale, landing, deploy Cloudflare, email routing public.

Cross-references utile în mobil: `PROJECT_HANDOFF.md` §7 (website și Cloudflare).

**Ce nu este în website repo:** cod Expo, SQL Supabase, Edge Functions, `app.json`, EAS config.

---

## Documente complementare (website repo)

| Fișier | Scop |
|--------|------|
| [`README.md`](../README.md) | Structură repo, previzualizare locală, deploy Git Cloudflare |
| `review_*.txt` | Review-uri task anterioare (ex. privacy policy) — neversionate în `.gitignore` |
