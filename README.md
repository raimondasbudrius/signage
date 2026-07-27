# 🖥️ Signage – Molėtų progimnazijos informacinis ekranas

Skaitmeninio informacinio ekrano (digital signage) platforma, rodanti informaciją iš kelių šaltinių pagal tvarkaraštį.

## 🌐 GitHub Pages (statinė versija – be serverio!)

Galite naudoti šią aplikaciją **be jokio serverio** – tik naršyklėje per GitHub Pages:

### Aktyvavimas

1. Eikite į GitHub repozitoriją: https://github.com/raimondasbudrius/signage
2. **Settings** → **Pages** → **Source** → pasirinkite `main` branch, aplanką `/docs`
3. Išsaugokite – po kelių minučių svetainė bus pasiekiama:
   - **Ekranas:** https://raimondasbudrius.github.io/signage/
   - **Admin:** https://raimondasbudrius.github.io/signage/admin.html

### Statinės versijos ypatybės

| Savybė | Serverinė versija | Statinė versija (GitHub Pages) |
|--------|-------------------|-------------------------------|
| Tvarkaraštis | ✅ SQLite | ✅ localStorage |
| Google Slides | ✅ | ✅ |
| Tvarkaraščio iframe | ✅ | ✅ |
| Facebook įrašai | ✅ (su token) | ❌ (CORS ribojimai) |
| Nustatymai | ✅ bendri visiems | ⚠️ tik toje naršyklėje |
| Reikia serverio | Taip | **Ne** |

> 💡 **Patarimas:** Atidarykite admin ir ekraną toje pačioje naršyklėje – pakeitimai admin'e automatiškai atsinaujins ekrane (per `localStorage` sinchronizaciją).

---

## Šaltiniai

| Šaltinis | Aprašymas |
|----------|-----------|
| 📋 **Tvarkaraštis** | Pamokų tvarkaraštis iš [raimondasbudrius.github.io/tvarkarastis](https://raimondasbudrius.github.io/tvarkarastis/) (rodomas per iframe) |
| 📊 **Google skaidrės** | Pranešimų skaidrės iš Google Slides (rodomos per iframe, automatiškai slenka) |
| 📘 **Facebook** | Naujausi įrašai (iki 3) su nuotraukomis iš [Mokykla.Moletai](https://www.facebook.com/Mokykla.Moletai) puslapio |

## Paleidimas (serverinė versija)

```bash
cd signage
npm install
npm start
```

Serveris pasileidžia ant `http://localhost:3000`.

> Jei norite naudoti be serverio, žr. **GitHub Pages** skyrių aukščiau.

## URL adresai

| Adresas | Paskirtis |
|---------|-----------|
| `http://localhost:3000/` | **Ekranas** – atidarykite per visą ekraną ant TV/monitoriaus |
| `http://localhost:3000/admin` | **Administravimas** – tvarkaraščio ir nustatymų valdymas |

## Administravimas

1. Atidarykite `/admin`
2. Prisijunkite su slaptažodžiu (pagal nutylėjimą: `admin123`)
3. **Tvarkaraštis** – nustatykite:
   - Kuriuos šaltinius rodyti
   - Laiko langą (nuo–iki) ir dienas
   - Rodymo trukmę sekundėmis
   - Eilės tvarką
4. **Nustatymai** – keiskite šaltinių URL, Facebook token, mokyklos pavadinimą

## Kaip veikia tvarkaraštis

- Kiekvienas šaltinis turi **laiko langą** (pvz., 07:00–17:00) ir **dienas** (Pr–Pn)
- Jei dabartinis laikas patenka į kelių šaltinių langus – jie rodomi **paeiliui**
- Kiekvienas šaltinis rodomas nustatytą **trukmę** (pvz., 120 sek.), tada perjungiamas
- Jei nėra aktyvaus šaltinio – rodomas **laukimo ekranas** su laikrodžiu
- Ekranas automatiškai atsinaujina kas 30 sek. (patikrina tvarkaraštį) ir kas 5 min. (patikrina nustatymus)

## Facebook API nustatymas

Norint rodyti Facebook įrašus, reikia **Page Access Token**:

1. Eikite į [Facebook Graph API Explorer](https://developers.facebook.com/tools/explorer/)
2. Pasirinkite puslapį „Mokykla.Moletai"
3. Sugeneruokite token su leidimais: `pages_read_engagement`, `pages_show_list`
4. Įklijuokite token į Admin → Nustatymai → „Facebook Access Token"

> ⚠️ Be token sistema veikia toliau – Facebook šaltinis tiesiog rodys pranešimą, kad įrašai nepasiekiami.

## Konfigūracija (.env)

Nukopijuokite `.env.example` į `.env` ir pakeiskite:

```env
PORT=3000
ADMIN_PASSWORD=admin123
SESSION_SECRET=pakeiskite-i-random-string
FACEBOOK_ACCESS_TOKEN=
```

## Technologijos

- **Node.js + Express** – serveris
- **SQLite** (better-sqlite3) – tvarkaraščio ir nustatymų saugykla
- **Vanilla JS** – ekrano ir admin frontendas (be framework'ų)

## Failų struktūra

```
signage/
├── server.js          # Express serveris + API
├── package.json
├── .env               # Konfigūracija
├── .env.example
├── .gitignore
├── public/            # Serverinės versijos frontend
│   ├── display.html   # Informacinis ekranas (kiosk)
│   └── admin.html     # Administravimo panelė
├── docs/              # GitHub Pages (statinė versija)
│   ├── index.html     # Informacinis ekranas (localStorage)
│   └── admin.html     # Administravimo panelė (localStorage)
└── signage.db         # SQLite duomenų bazė (sukuriama automatiškai)
```
