# Vinyl Radar

Persoonlijke koopjes-radar voor tweedehands funk / soul / cosmic / dub / psych én
heavy/stoner vinyl. Een cloud-agent draait 2×/week, zoekt Discogs marketplace + Marktplaats
af op basis van [`BRIEF.md`](BRIEF.md), en voegt nieuwe vondsten toe aan `data.js`.

**Interface:** open `index.html` (of de GitHub Pages-URL). Filter op categorie (A DJ /
B thuis / C crate-digging), bak, prijs en wildcards; wishlist wordt lokaal bewaard.

- `index.html` — de interface
- `data.js` — de platen (seed-aanbevelingen + live vondsten)
- `BRIEF.md` — smaakprofiel & zoekregels
- `AGENT.md` — instructies voor de cloud-agent
- `owned.json` — reeds bezeten platen (dedup)
