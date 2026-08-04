# Poletuška 🐝

Klidná „flutter" hra o včelce, která poletuje krajinou a sbírá pyl rozmanitých
tvarů, jak se střídají roční období. Žádná prohra — jen leť a objevuj.

Celá hra je **jeden soubor** `poletuska.html`. Otevři ho v prohlížeči a hraj.

## Jak se hraje

- Veď včelku **myší / prstem** (nebo **šipkami / WASD**).
- Přibliž se ke květu — **pyl si sám přiletí** (magnet + jiskřičky).
- **Deník** (vpravo nahoře) loguje každý nalezený druh jako specimen.
- Přepínač **den / noc** vlevo dole.

## Co je na tom zajímavé

- **12 druhů pylu kreslených podle reálné palynologie** — ostnitá (echinate),
  síťovaná (reticulate), třípórová, tříbrázdá (tricolpate), tetráda, se
  vzdušnými vaky (saccate). Každý druh má český i latinský název skutečné
  rostliny.
- **4 roční období** se střídají a mění oblohu, květy i dostupné druhy pylu.
- Vše se vykresluje **procedurálně na canvas** — hra běží bez jakýchkoli
  externích souborů.

## Vlastní grafika (nepovinné)

Hra umí místo vektorové kresby použít PNG obrázky. Stačí je uložit do
`assets/` pod správným názvem (`bee.png`, `pollen_taraxacum.png`,
`flower_papaver.png` …) — hra si jich automaticky všimne, chybějící soubor
znamená fallback na kresbu.

- `assets/manifest.json` — seznam všech 25 assetů
- `assets/PROMPTS.md` — hotové ComfyUI prompty (SDXL, průhledné pozadí)
- `PLAN.md` — plán generování assetů přes ComfyUI na Apple Silicon

## Technikálie

Čistý HTML + Canvas 2D, bez závislostí a build kroku. Podporuje dotyk,
klávesnici, světlý/tmavý motiv a `prefers-reduced-motion`.

## Licence

MIT
