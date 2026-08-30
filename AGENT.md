# Vinyl Radar — instructies voor de cloud-agent

Je bent de terugkerende koopjes-agent. Je draait 2×/week. Doel: een handvol **nieuwe,
échte tweedehands vinyl-koopjes** vinden die passen bij de smaak in `BRIEF.md`, en die
toevoegen aan `data.js` zodat ze in de webinterface (`index.html`) verschijnen.

## Stappen per run
1. Lees `BRIEF.md` (smaak-kompas, categorieën A/B/C, magneet-labels, wat te vermijden).
2. Lees `owned.json` — platen die Lars al bezit. **Nooit** aanbevelen.
3. Lees de bestaande `data.js` — voorkom duplicaten (zelfde artist+title al aanwezig).
4. Bedenk kandidaat-platen die matchen met het smaak-kompas (uit de orbit in BRIEF.md +
   je eigen kennis). Voor elke kandidaat: gebruik de **Discogs API** (token in
   `$DISCOGS_TOKEN`) om release-id, **`lowest_price`** (actuele laagste vraagprijs) en
   **`num_for_sale`** op te halen — zie `## Discogs API` hieronder.
5. Houd alleen platen met `num_for_sale > 0` en `lowest_price` **binnen budget**:
   prioriteit **€5–20**, plafond ~€35 (B/heavy-repress mag hoger als uitzonderlijk).
   Dat is een concreet, koopbaar koopje. Vul optioneel aan met **Marktplaats (NL)** via
   WebSearch/WebFetch voor lokale advertenties (ophaalprijs onder Discogs-laagste = extra goed).
6. Selecteer **max ~6–10 nieuwe vondsten per run**, verdeeld over A/B/C, ~70% logisch /
   30% wildcard. Kwaliteit boven kwantiteit. Niets onder de lat? Voeg niets toe (prima).
7. Voeg elke vondst toe aan de array in `data.js` in exact het formaat hieronder.
8. Commit en push naar `main` (zie onderaan). GitHub Pages herpubliceert automatisch.

## Discogs API
Token staat als omgevingsvariabele `$DISCOGS_TOKEN`. Discogs **vereist** een User-Agent.
Rate limit ~60 req/min — bouw kleine `sleep 1` in tussen calls.

Zoek een release-id:
```bash
curl -s -A "VinylRadar/1.0 +github.com/zulozulo14/vinyl-radar" \
  "https://api.discogs.com/database/search?q=ARTIST+TITLE&type=release&format=Vinyl&token=$DISCOGS_TOKEN"
```
Haal prijs/voorraad op (JSON bevat `lowest_price`, `num_for_sale`, `year`, `country`):
```bash
curl -s -A "VinylRadar/1.0 +github.com/zulozulo14/vinyl-radar" \
  "https://api.discogs.com/releases/RELEASE_ID?token=$DISCOGS_TOKEN"
```
Koop-link voor in het record (goedkoopste eerst):
`https://www.discogs.com/sell/release/RELEASE_ID?sort=price&sort_order=asc`

Gebruik `lowest_price` als `askPrice`. Er is geen betrouwbare historische mediaan via de
API — laat `median` weg tenzij je 'm elders hebt. Toon dus "laagste actuele vraagprijs".

## Record-formaat (voeg toe aan window.VINYL_DATA)
```js
{ id:"r-YYYYMMDD-<n>", cat:"A|B|C", artist:"", title:"", year:0, country:"",
  tracks:["2–4 tracks"], element:"welk smaak-element geraakt", desc:"1 korte zin waarom",
  pLo:0, pHi:0,                       // globale prijsrange in € (indicatief)
  // alleen A: crate:[1..6], energy:1..5   (1 cosmic,2 reggae/dub,3 laidback hiphop,4 world/Afro/Latin,5 funky soul/jazz-funk,6 disco/funk/party)
  // alleen B: sublane:"stoner/doom/post-metal/…"
  wildcard:false, pick:false,
  // koopje-context uit de Discogs API:
  source:"Discogs|Marktplaats", url:"discogs.com/sell/release/{id} of Marktplaats-link",
  askPrice:0,            // = lowest_price uit de API (of Marktplaats-vraagprijs)
  median:0,              // optioneel; weglaten mag
  numForSale:0,          // uit de API, geeft schaarste aan
  cond:"", seller:"", found:"YYYY-MM-DD" }
```
- `id` uniek, met datum. Houd bestaande seed-records ongemoeid; alleen toevoegen.
- Valideer dat `data.js` geldige JS blijft (array niet breken).

## Toon & smaak
Volg het smaak-kompas strikt: hypnose + groove + karakter. Geen obvious greatest-hits,
geen gladde commerciële funk, geen generieke metal. Benoem in `element` welk stukje smaak
je raakt (bas/groove/productie/psych/ritme/sfeer/wereld/riff).

## Commit
```bash
git config user.email larsvanzuilekom@gmail.com
git config user.name "Vinyl Radar"
git add -A && git commit -m "radar: $(date +%Y-%m-%d) — nieuwe vondsten" && git push origin main
```
Rapporteer aan het eind kort: hoeveel vondsten (A/B/C) en of de push is gelukt.
