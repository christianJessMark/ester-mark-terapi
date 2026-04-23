# Ester Mark Terapi – Hjemmeside

Hugo-baseret hjemmeside med Decap CMS til indholdsstyring.

---

## Opsætning (gøres én gang)

### 1. Opret GitHub-repo
1. Gå til github.com → "New repository"
2. Navn: `ester-mark-terapi`
3. Sæt til **Public** (påkrævet for gratis GitHub Pages)
4. Klik "Create repository"

### 2. Upload filerne
```bash
git init
git add .
git commit -m "Første upload"
git remote add origin https://github.com/DIT-BRUGERNAVN/ester-mark-terapi.git
git push -u origin main
```

### 3. Ret config-filen
I `static/admin/config.yml` — ret denne linje:
```yaml
repo: DIT-GITHUB-BRUGERNAVN/ester-mark-terapi
```

### 4. Slå GitHub Pages til
1. Repo → Settings → Pages
2. Source: **GitHub Actions**
3. Gem

Sitet er nu live på: `https://DIT-BRUGERNAVN.github.io/ester-mark-terapi/`

### 5. Peg domæne på GitHub Pages
I dit domænes DNS-indstillinger:
```
Type: CNAME
Navn: www
Værdi: DIT-BRUGERNAVN.github.io
```
Tilføj også i Settings → Pages → Custom domain: `estermarkterapi.dk`

### 6. Sæt kontaktformular op (Formspree)
1. Gå til formspree.io → opret gratis konto
2. Opret nyt form → kopiér form-ID (fx `xpzgkqrw`)
3. Indsæt i `hugo.toml`:
```toml
formspree_id = "xpzgkqrw"
```

### 7. Sæt Decap CMS op (din kones editor)
1. Gå til github.com → Settings → Developer settings → OAuth Apps → New
2. Application name: `Decap CMS`
3. Homepage URL: `https://estermarkterapi.dk`
4. Callback URL: `https://api.netlify.com/auth/done`
5. Kopiér Client ID og Client Secret
6. Gå til netlify.com → opret gratis konto → Site configuration → Access & security → OAuth
7. Tilføj GitHub provider med dit Client ID og Secret

Din kone logger herefter ind på:
`https://estermarkterapi.dk/admin/`

---

## Daglig brug (din kone)

1. Gå til `estermarkterapi.dk/admin`
2. Log ind med GitHub
3. Klik "Forsideindhold"
4. Ret tekst → klik "Gem"
5. Sitet opdateres automatisk inden for ~1 minut

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
│   └── admin/
│       ├── index.html     ← Decap CMS
│       └── config.yml     ← Definerer hvad der kan redigeres
├── .github/workflows/
│   └── deploy.yml         ← Auto-deploy ved hvert gem
└── hugo.toml              ← Konfiguration
```
