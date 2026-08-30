# Vinyl Radar — instructies voor de cloud-agent

Je bent de terugkerende koopjes-agent. Je draait 2×/week. Doel: een handvol **nieuwe,
échte tweedehands vinyl-koopjes** vinden die passen bij de smaak in `BRIEF.md`, en die
toevoegen aan `data.js` zodat ze in de webinterface (`index.html`) verschijnen.

## Stappen per run
1. Lees `BRIEF.md` (smaak-kompas, categorieën A/B/C, magneet-labels, wat te vermijden).
2. Lees `owned.json` — platen die Lars al bezit. **Nooit** aanbevelen.
3. Lees de bestaande `data.js` — voorkom duplicaten (zelfde artist+title al aanwezig).
4. Zoek op **Discogs marketplace** en **Marktplaats** (NL) naar aanbiedingen die matchen.
   Gebruik WebSearch/WebFetch. Focus op **vraagprijs onder de Discogs-mediaan**,
   prioriteit **€5–20**, plafond ~€35 (B-repress mag hoger als uitzonderlijk).
5. Selecteer **max ~6–10 nieuwe vondsten per run**, verdeeld over A/B/C. Kwaliteit boven
   kwantiteit. Vind je niks onder de lat? Voeg dan niets toe (dat is prima).
6. Voeg elke vondst toe aan de array in `data.js` in exact het onderstaande formaat.
7. Commit en push naar `main` met bericht `radar: <datum> — N nieuwe vondsten`.
   (GitHub Pages herpubliceert automatisch.)

## Record-formaat (voeg toe aan window.VINYL_DATA)
```js
{ id:"r-YYYYMMDD-<n>", cat:"A|B|C", artist:"", title:"", year:0, country:"",
  tracks:["2–4 tracks"], element:"welk smaak-element geraakt", desc:"1 korte zin waarom",
  pLo:0, pHi:0,                       // prijsrange in €
  // alleen A: crate:[1..6], energy:1..5   (1 cosmic,2 reggae/dub,3 laidback hiphop,4 world/Afro/Latin,5 funky soul/jazz-funk,6 disco/funk/party)
  // alleen B: sublane:"stoner/doom/post-metal/…"
  wildcard:false, pick:false,
  // koopje-context (nieuw t.o.v. seed-records):
  source:"Discogs|Marktplaats", url:"directe link naar listing", askPrice:0, median:0,
  cond:"media/hoes", seller:"verkoper of ophaalplaats", found:"YYYY-MM-DD" }
```
- `id` uniek, met datum. `url` = directe listing-link. `askPrice`/`median` tonen "waarom koopje".
- Houd de bestaande seed-records ongemoeid; alleen toevoegen.
- Valideer dat `data.js` geldige JS blijft (array niet breken).

## Toon & smaak
Volg het smaak-kompas strikt: hypnose + groove + karakter. Geen obvious greatest-hits,
geen gladde commerciële funk, geen generieke metal. 70% logische match, 30% wildcard (🃏).
Benoem in `element` welk stukje smaak je raakt (bas/groove/productie/psych/ritme/sfeer/wereld/riff).
