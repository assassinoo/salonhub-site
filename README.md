# SalonHub — website public

Acest repository conține site-ul web static public pentru **SalonHub** — platforma pentru descoperirea saloanelor și gestionarea programărilor.

Conținutul este HTML și CSS pur, fără framework-uri sau build tools. Este separat de aplicația mobilă Expo.

## Structură

| Fișier       | Descriere                          |
| ------------ | ------------------------------------ |
| `index.html` | Pagina principală de landing         |
| `styles.css` | Stiluri responsive                   |

## Previzualizare locală (Windows)

Deschide pagina în browserul implicit:

```powershell
Start-Process .\index.html
```

Alternativ, deschide manual `index.html` din Explorer sau servește fișierele cu orice server static local.

## Deployment

Ținta de deployment este **Cloudflare Pages**. Fișierele din rădăcina repository-ului pot fi publicate direct, fără pas de build.

Domeniul planificat este `salonhub.ro` (configurarea DNS și validarea vor fi făcute separat).
