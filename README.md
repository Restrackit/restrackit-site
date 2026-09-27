# restrackit-site

Landing page statica di Restrackit (https://restrackit.it). HTML + CSS, nessuna build.
Tutto ciò che viene pubblicato sta in `site/`; il workflow `.github/workflows/pages.yml` la carica su GitHub Pages a ogni push su `main`.

Anteprima locale:

```bash
python3 -m http.server 8080 --bind 127.0.0.1 --directory site
```

## Prima della pubblicazione (TODO)

- [x] Google Form: https://forms.gle/BkNGwGhaoGwES6xX8
      Domande: nome locale/gruppo, tipo (ristorante, catena/franchising, mensa/catering, bar/punto ristoro), numero di locali,
      città, come registri oggi lotti e scadenze, email e telefono, consenso privacy (link a https://restrackit.it/privacy.html).
- [ ] Sostituire le due anteprime HTML di "Come funziona" con uno screenshot reale dell'app (nuovo lotto) e la foto di un'etichetta stampata.
- [ ] Creare la casella `info@restrackit.it` su Zoho Mail (vedi sotto).
- [ ] Quando i 5 posti pilota sono assegnati: bottone "Entra in lista d'attesa".
- [ ] Con la partita IVA: aggiungerla al footer e aggiornare il titolare in `privacy.html`.

## Pubblicazione

1. Comprare `restrackit.it` su Route 53 (Registered domains → Register). La hosted zone viene creata in automatico.
2. Creare il repo `restrackit-site` nell'org GitHub Restrackit (pubblico: GitHub Pages sul piano Free lo richiede) e fare push.
3. Repo → Settings → Pages → Source: **GitHub Actions**. Custom domain: `restrackit.it`.
4. Org → Settings → Pages → **Verify** `restrackit.it` (record TXT `_github-pages-challenge-restrackit` indicato da GitHub), per evitare takeover del dominio.
5. Record su Route 53 (hosted zone `restrackit.it`):

   | Nome | Tipo | Valore |
   |---|---|---|
   | `restrackit.it` | A | `185.199.108.153` `185.199.109.153` `185.199.110.153` `185.199.111.153` |
   | `restrackit.it` | AAAA | `2606:50c0:8000::153` `2606:50c0:8001::153` `2606:50c0:8002::153` `2606:50c0:8003::153` |
   | `www.restrackit.it` | CNAME | `restrackit.github.io` |

6. Quando il certificato è pronto: Settings → Pages → **Enforce HTTPS**.
7. Google Search Console e Bing Webmaster Tools: aggiungere la proprietà di dominio (record TXT) e inviare `https://restrackit.it/sitemap.xml`.

## Posta: Zoho Mail (piano Forever Free, data center EU)

Registrarsi da https://www.zoho.eu/mail/ (dati in UE), aggiungere il dominio e creare `info@restrackit.it`. Record su Route 53:

| Nome | Tipo | Valore |
|---|---|---|
| `restrackit.it` | TXT | codice di verifica `zoho-verification=...` fornito da Zoho |
| `restrackit.it` | MX | `10 mx.zoho.eu` `20 mx2.zoho.eu` `50 mx3.zoho.eu` |
| `restrackit.it` | TXT | `"v=spf1 include:zohomail.eu ~all"` |
| `zmail._domainkey.restrackit.it` | TXT | chiave DKIM generata nel pannello Zoho |
| `_dmarc.restrackit.it` | TXT | `"v=DMARC1; p=none; rua=mailto:info@restrackit.it"` |

Nota: su Route 53 i due TXT sull'apex (verifica Zoho e SPF) vanno nello **stesso** record, uno per riga.
