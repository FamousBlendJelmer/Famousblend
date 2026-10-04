# Famous Blend — vooraankondigingssite

Een statische één-pagina website (vooraankondiging) met contactformulier.
Gemaakt om te hosten op **GitHub Pages** met je eigen domeinnaam.

## Inhoud

```
famous-blend-site/
├─ index.html            ← de website (pas hierin je e-mailadres aan)
├─ .nojekyll             ← zorgt dat GitHub de map 1-op-1 serveert
├─ README.md             ← dit bestand
└─ assets/
   ├─ logo.png           ← logo in espresso (hero, op crème achtergrond)
   ├─ logo-cream.png     ← logo in crème (footer, op donkere achtergrond)
   ├─ favicon.png        ← tabblad-icoon 256px
   └─ favicon-32.png     ← tabblad-icoon 32px
```

Het logo (blender met microfoon + "FAMOUS BLEND") staat in de hero en de footer,
ingekleurd in de merkkleuren.

---

## Stap 1 — Contactformulier instellen (1 minuut)

Het formulier gebruikt **Web3Forms**: gratis, geen account nodig, werkt op
GitHub Pages (dat zelf geen formulieren kan verwerken). Belangrijk: **je
e-mailadres staat nergens in de code** en is dus niet zichtbaar in de broncode
in de browser. Het formulier gebruikt in plaats daarvan een anonieme *access key*.

1. Ga naar https://web3forms.com en vul het e-mailadres in waar de berichten
   heen moeten (bv. `info@famousblend.nl`). Je krijgt een **access key** (een
   lange code) gemaild. Geen account nodig.
2. Open `index.html`, zoek (Ctrl/Cmd + F) op `WEB3FORMS_ACCESS_KEY` en vervang
   die door de code uit de mail. Opslaan.

> Je e-mailadres is alleen bij Web3Forms bekend (op hun server), gekoppeld aan
> de key. In de website zelf is het nergens te zien. Spam wordt tegengehouden
> met een verborgen honeypot-veld.

### Waarom geen PHP?
GitHub Pages serveert alleen statische bestanden en **draait geen PHP** — een
PHP-mailscript zou daar niet werken. De Web3Forms-aanpak hierboven haalt
hetzelfde doel (adres onzichtbaar) zonder server. Wil je later tóch een eigen
PHP-oplossing, dan moet de site op een host met PHP staan (geen GitHub Pages);
vraag er gerust naar.

---

## Stap 2 — Op GitHub plaatsen

### Makkelijkste manier (via de website, geen commando's)

1. Ga naar https://github.com → **New repository**.
2. Naam: bijvoorbeeld `famous-blend-site`. Zet 'm op **Public**. → **Create**.
3. Op de nieuwe repo-pagina: **Add file → Upload files**.
4. Sleep de **inhoud** van deze map erin (dus `index.html`, `.nojekyll`,
   `README.md` en de map `assets`) — niet de bovenliggende map zelf.
5. Onderaan: **Commit changes**.

### Of via de command line (als je git gebruikt)

```bash
cd famous-blend-site
git init
git add .
git commit -m "Famous Blend vooraankondiging"
git branch -M main
git remote add origin https://github.com/JOUW-GEBRUIKERSNAAM/famous-blend-site.git
git push -u origin main
```

---

## Stap 3 — GitHub Pages aanzetten

1. In de repo: **Settings → Pages**.
2. Onder **Build and deployment → Source**: kies **Deploy from a branch**.
3. Branch: **main**, map: **/ (root)**. → **Save**.
4. Wacht ~1 minuut. Bovenaan verschijnt je tijdelijke adres:
   `https://JOUW-GEBRUIKERSNAAM.github.io/famous-blend-site/`
   Controleer daar of alles goed staat.

---

## Stap 4 — Je eigen domein koppelen

1. **Settings → Pages → Custom domain**: vul je domein in.
   - Kies één hoofdvorm. Aanbevolen: `www.JOUWDOMEIN.nl`
     (of de kale vorm `JOUWDOMEIN.nl` — zie DNS hieronder).
   - Klik **Save**. GitHub maakt nu automatisch een `CNAME`-bestand in de repo.
2. Vink daarna **Enforce HTTPS** aan (kan pas nadat DNS klopt; zie stap 5).

---

## Stap 5 — DNS instellen bij je domeinregistrar

Log in bij de partij waar je het domein hebt geregistreerd en open het
**DNS-beheer**. Je hebt twee smaken; **doe bij voorkeur allebei**, dan werkt
zowel `JOUWDOMEIN.nl` als `www.JOUWDOMEIN.nl`.

### A) Kale domein (apex, `JOUWDOMEIN.nl`) → 4 A-records

| Type | Naam (host) | Waarde            |
|------|-------------|-------------------|
| A    | `@`         | `185.199.108.153` |
| A    | `@`         | `185.199.109.153` |
| A    | `@`         | `185.199.110.153` |
| A    | `@`         | `185.199.111.153` |

Optioneel ook IPv6 (AAAA), zelfde `@`:
`2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`

### B) www-subdomein (`www.JOUWDOMEIN.nl`) → 1 CNAME

| Type  | Naam (host) | Waarde                        |
|-------|-------------|-------------------------------|
| CNAME | `www`       | `JOUW-GEBRUIKERSNAAM.github.io.` |

> Let op:
> - Zet bij "Naam/host" vaak gewoon `@` (= het kale domein) en `www`.
>   Sommige registrars willen de volledige naam; volg hun voorbeeld.
> - Verwijder oude, botsende records (bijv. een bestaande A of een
>   "parking"/forwarding-record) voor `@` en `www`.
> - Heb je een bestaand `CNAME` op `@`? Dat mag niet. Gebruik dan de A-records.

DNS-wijzigingen zijn meestal binnen een uur actief, soms duurt het tot 24 uur.

---

## Stap 6 — Controleren

- Open `https://JOUWDOMEIN.nl` en `https://www.JOUWDOMEIN.nl`.
- In **Settings → Pages** moet er een groen vinkje bij het domein staan.
- Zet **Enforce HTTPS** aan zodra dat kan.
- Check de DNS-status eventueel op https://dnschecker.org.

---

## Later iets wijzigen

Pas `index.html` aan en upload/commit het opnieuw naar de repo — binnen een
minuut staat de wijziging live. Het `CNAME`-bestand en je DNS blijven staan.

---

### Handig om te weten
- GitHub Pages is gratis voor publieke repo's.
- De site is volledig statisch (geen server). Het formulier loopt daarom via
  FormSubmit; wil je later iets anders (Mailchimp, eigen nieuwsbrief), dan is
  dat een kwestie van het `<form>`-blok vervangen.
- Officiële GitHub-handleiding voor domeinen:
  https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site
