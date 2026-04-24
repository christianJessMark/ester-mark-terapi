# Ester Mark Terapi – Hjemmeside

Hugo-baseret hjemmeside hostet på Cloudflare Workers med adgangsbeskyttelse via Cloudflare Access.

---

## Arkitektur

```
GitHub repo → Cloudflare bygger Hugo → Cloudflare Workers server sitet → Cloudflare Access beskytter med email-login
```

- **Kode**: GitHub (`christianJessMark/ester-mark-terapi`)
- **Hosting**: Cloudflare Workers
- **Adgangsbeskyttelse**: Cloudflare Zero Trust Access (engangskode på email)
- **Domæne**: estermarkterapi.dk (DNS styret af Cloudflare)

---

## Adgang til sitet

Sitet er beskyttet. For at få adgang:

1. Gå til `https://estermarkterapi.dk`
2. Skriv din email-adresse
3. Tjek din indbakke for en engangskode fra Cloudflare
4. Indtast koden

Kun emails der er godkendt i Cloudflare Access kan logge ind. For at tilføje en ny bruger, gå til Cloudflare Zero Trust → Access controls → Applications → estermarkterapi.dk → Configure → Tilladt adgang.

---

## Rediger indhold

Al tekst på sitet ligger i én fil: `content/_index.md`

Ret direkte i filen på GitHub, eller brug Decap CMS på `https://estermarkterapi.dk/admin/` (kræver opsætning — se nedenfor).

Når du gemmer, bygger Cloudflare automatisk sitet inden for ~1 minut.

---

## Struktur

```
ester-mark/
├── content/
│   └── _index.md          ← ALT tekst på sitet ligger her
├── layouts/
│   ├── _default/baseof.html
│   └── index.html         ← HTML-skabelon
├── static/
│   ├── css/main.css       ← Al CSS
│   ├── uploads/           ← Billeder
│   ├── favicon.svg        ← Favicon
│   └── admin/
│       ├── index.html     ← Decap CMS
│       └── config.yml     ← Definerer hvad der kan redigeres
├── wrangler.jsonc         ← Cloudflare Workers konfiguration
└── hugo.toml              ← Hugo konfiguration
```

---

## Opsætning fra bunden (hvis nødvendigt)

### 1. GitHub repo

```bash
git init
git add .
git commit -m "Første upload"
git remote add origin https://github.com/christianJessMark/ester-mark-terapi.git
git push -u origin main
```

### 2. Cloudflare Workers

1. Gå til cloudflare.com → Workers & Pages → Create application
2. Vælg Continue with GitHub → vælg repo
3. Build command: `hugo --minify`
4. Tilføj environment variable: `HUGO_VERSION` = `0.145.0`
5. Deploy

Tilføj custom domain under Settings → Domains & Routes → Add → `estermarkterapi.dk`

### 3. Cloudflare DNS

Domænet skal pege på Cloudflare navneservere. Sæt disse hos Simply.com:

```
houston.ns.cloudflare.com
tara.ns.cloudflare.com
```

### 4. Cloudflare Access (adgangsbeskyttelse)

1. Cloudflare → Zero Trust → Access controls → Applications → Add an application → Self-hosted
2. Domain: `estermarkterapi.dk`
3. Opret policy med Action: Allow, Selector: Emails, tilføj godkendte emailadresser
4. Gem

### 5. Kontaktformular (Formspree)

1. Gå til formspree.io → opret gratis konto
2. Opret nyt form → kopiér form-ID
3. Indsæt i `hugo.toml`:

```toml
formspree_id = "DIT-FORM-ID"
```
