# Færø-kortet 🗺 (privat ferieprojekt — juli 2026)

Interaktivt kort over Færøerne med alle tips/attraktioner samlet fra rejseguides
og personlige anbefalinger. Base: Vestmanna (bor i bilen), transport: bil.

**Åbn kortet:** dobbeltklik på `index.html` (virker direkte fra disk, kræver kun internet til kort-tiles).

## Struktur

| Fil | Rolle |
|-----|-------|
| `index.html` | Hele kort-appen (Leaflet + clean HTML/CSS/JS, ingen build-step) |
| `data/attractions.js` | **Eneste datakilde** — alle punkter som JS-array (`window.FAROE_ATTRACTIONS`) |
| `README.md` | Denne fil — intake-processen for nye tips |

"Set"-status gemmes i browserens localStorage (nøgle `faroe-visited-v1`) og kan
desuden persisteres i datafilen via feltet `visited: true` (knappen "Eksportér
status" i appen genererer opdateret JSON til indsættelse).

## STANDARDISERET INTAKE — sådan tilføjer en bot (eller menneske) et nyt tip

Når brugeren skriver et nyt tip (fx *"Vi har fået anbefalet kaffebaren i Gjógv"*),
skal du **kun** gøre følgende:

1. **Find manglende info selv** (websøgning er tilladt og forventet):
   - Præcise koordinater (WGS84 decimal, 5 decimaler — slå op, gæt aldrig)
   - Ø + kategori + evt. praktiske tips (adgang, hiking-gebyr, sæson, færge/tunnel)
2. **Tjek for dubletter** i `data/attractions.js` (søg på navn OG på stednavne i beskrivelsen).
   Findes punktet: tilføj kilden til `sources` og flet tips — opret ALDRIG en dublet.
3. **Tilføj ét objekt** til arrayet i `data/attractions.js` efter skemaet nedenfor. Intet andet skal ændres — kortet samler selv op.
4. Hold `id` stabilt for evigt (bruges som localStorage-nøgle for "set"-status).

### Skema (alle felter)

```js
{
  id: "kebab-case-unikt-navn",     // PÅKRÆVET, stabilt, aldrig genbrugt
  name: "Mulafossur",              // PÅKRÆVET, det lokale navn
  island: "Vágar",                 // PÅKRÆVET: Streymoy|Vágar|Eysturoy|Borðoy|Kalsoy|Viðoy|Kunoy|Mykines|Nólsoy|Sandoy|Suðuroy|Koltur|Hestur|Svínoy|Fugloy|Skúvoy|Stóra Dímun
  lat: 62.11778, lng: -7.44306,    // PÅKRÆVET, WGS84 decimal
  category: "natur",               // PÅKRÆVET: natur|vandretur|landsby|udsigt|kultur|mad|aktivitet|fugle
  description: "1-2 sætninger på dansk.",   // PÅKRÆVET
  tips: "Praktisk: gebyr, parkering, timing…", // valgfri, "" hvis intet
  image: "https://upload.wikimedia.org/…",     // valgfri: KUN Wikimedia Commons-thumb (stabil + fri licens; aldrig hotlink fra Google/private sites). Udelad hellere end at gætte.
  imagePage: "https://commons.wikimedia.org/wiki/File%3A…", // valgfri: Commons-filside (kredit/licens), følges med image
  sources: ["guidetofaroeislands.fo", "bruger-tip 2026-07-08"], // PÅKRÆVET, min. 1
  mustSee: false,                  // true = fremhævet med stjerne
  visited: false                   // styres normalt fra appen
}
```

### Kategorier → farve/ikon (defineret i index.html, tilføj IKKE nye uden grund)

`natur` grøn · `vandretur` orange · `landsby` blå · `udsigt` lilla ·
`kultur` brun · `mad` rød · `aktivitet` teal · `fugle` cyan

## Kilder indsamlet 2026-07-07

- guidetofaroeislands.fo (11 best attractions)
- danny-cph.com (things to do)
- visitfaroeislands.com (tips from travellers)
- magnificentworld.com (things to do)
- heritagewanderlust.com (3 days guide)
