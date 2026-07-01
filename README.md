# SalonHub — website public

Acest repository conține site-ul web static public pentru **SalonHub** — platforma pentru descoperirea saloanelor și gestionarea programărilor.

Conținutul este HTML și CSS pur, fără framework-uri sau build tools. Este separat de aplicația mobilă Expo (`D:\SalonHub\salonhub-mobile`).

**Pentru handoff complet (repo vs production, pagini legale, email, next steps):**

👉 **[`docs/WEBSITE_HANDOFF.md`](docs/WEBSITE_HANDOFF.md)**

## Structură

| Fișier          | Descriere                                      |
| --------------- | ---------------------------------------------- |
| `index.html`    | Pagina principală de landing                     |
| `styles.css`    | Stiluri responsive                             |
| `worker.js`     | Worker minimal pentru servirea asset-urilor    |
| `wrangler.jsonc`| Configurare Cloudflare Workers Static Assets   |

## Previzualizare locală (Windows)

Deschide pagina în browserul implicit:

```powershell
Start-Process .\index.html
```

Alternativ, deschide manual `index.html` din Explorer sau servește fișierele cu orice server static local.

## Deployment (Cloudflare Workers Static Assets)

Site-ul se publică prin **Cloudflare Workers Static Assets**, cu deploy din GitHub prin fluxul Worker Git din Cloudflare Dashboard.

Fișierele statice (`index.html`, `styles.css`) sunt servite din rădăcina repository-ului. `worker.js` le expune prin binding-ul `ASSETS`. Fișierele de configurare, documentația și review-urile sunt excluse la upload prin `.assetsignore`.

### Setări Git în Cloudflare Dashboard

| Câmp            | Valoare           |
| --------------- | ----------------- |
| Build command   | `npm ci`          |
| Deploy command  | `npm run deploy`  |

### Deploy manual (opțional)

```powershell
npm ci
npm run deploy
```

Domeniul public, redirect-ul `www` și starea live sunt documentate în [`docs/WEBSITE_HANDOFF.md`](docs/WEBSITE_HANDOFF.md) (secțiunea 2). Configurarea DNS și domeniului custom se face în Cloudflare Dashboard, separat de fișierele HTML.
