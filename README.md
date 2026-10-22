# Ester Mark Terapi – Hjemmeside

Hugo-baseret hjemmeside deployet til GitHub Pages med adgangsbeskyttelse via Cloudflare Access.

---

## Arkitektur

```
GitHub repo → GitHub Actions bygger Hugo → GitHub Pages server sitet → Cloudflare Access beskytter med email-login
```

- **Kode**: GitHub (`christianJessMark/ester-mark-terapi`)
- **Hosting**: GitHub Pages (bygget og deployet med GitHub Actions)
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

Når du gemmer, opretter Decap CMS et commit på GitHub. GitHub Actions bygger og deployer derefter sitet automatisk.

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

### 6. Decap CMS (redigering af indhold)

Decap CMS bruger GitHub til at gemme ændringer. Netlify bruges kun som OAuth-loginservice; sitet hostes fortsat på GitHub Pages. Brugeren, der skal redigere indhold, skal have skriveadgang til GitHub-repoet og være godkendt i Cloudflare Access.

1. Opret en OAuth App på GitHub under **Settings → Developer settings → OAuth Apps → New OAuth App**.
2. Udfyld:
   - **Application name**: `Decap CMS`
   - **Homepage URL**: `https://estermarkterapi.dk`
   - **Authorization callback URL**: `https://api.netlify.com/auth/done`
3. Opret appen, kopiér **Client ID**, og opret en **Client Secret**. Gem den sikkert; den kan ikke vises igen.
4. Åbn Netlify-projektet, der skal bruges til OAuth-konfigurationen. Netlify-projektet behøver ikke hoste hjemmesiden.
5. Gå til **Project configuration → Security → OAuth**.
6. Under **Authentication Providers**, vælg **Install Provider → GitHub**, og indsæt Client ID og Client Secret. Gem.
7. Giv redaktøren skriveadgang til GitHub-repoet (`christianJessMark/ester-mark-terapi`).

Netlifys menuplacering kan ændre sig. Den aktuelle vejledning findes i [Netlifys dokumentation om OAuth-provider tokens](https://docs.netlify.com/manage/security/secure-access-to-sites/oauth-provider-tokens/).

### Daglig brug

1. Gå til `https://estermarkterapi.dk/admin/`.
2. Log ind gennem Cloudflare Access, hvis du bliver bedt om det.
3. Vælg **Login with GitHub**, og godkend adgang.
4. Klik **Forsideindhold**, ret teksten, og klik **Gem**.
5. GitHub Actions bygger og udgiver den opdaterede hjemmeside automatisk.
