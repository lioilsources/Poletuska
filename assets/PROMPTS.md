# Assety pro Poletušku — ComfyUI prompt-sheet

Hra funguje i **bez** assetů (kreslí pyl vektorově). Jakmile do téhle složky
uložíš PNG se správným názvem (viz `manifest.json`), hra ho automaticky použije.

- **Formát:** čtvercové PNG, **průhledné pozadí**, doporučeně **1024×1024**
  (hra si to zmenší). Motiv **na střed**, kolem trocha místa.
- **Názvy:** přesně podle `manifest.json`, např. `pollen_taraxacum.png`,
  `flower_papaver.png`, `bee.png`.

---

## Doporučené nastavení (ComfyUI)

- **Model:** SDXL checkpoint (např. *Juggernaut XL*, *DreamShaper XL*).
- **Rozlišení:** 1024×1024, **Steps** 30, **CFG** 6–7, sampler **dpmpp_2m** / karras.
- **Průhlednost:** vygeneruj na **plochém pozadí** (níže v promptu) a odmázni ho
  nodem **`Image Remove Background (rembg)`** nebo **`InspyrenetRembg`** →
  `SaveImage` (PNG s alfou). Alternativa: model **LayerDiffuse** (rovnou RGBA).
- Pro ostrý mikroskopický vzhled pomáhá LoRA/styl „SEM / electron microscope“.

> Pozn.: nevím jistě, co znamenalo **„SPSRK“** — jestli je to konkrétní
> checkpoint nebo workflow, napiš mi jeho jméno a prompty pod něj doladím.

### Společný negativní prompt

```
text, watermark, signature, label, letters, blurry, out of frame, cropped,
multiple objects, duplicate, low quality, jpeg artifacts, hands, people
```

---

## 1) Včelka — `bee.png`

Pohled shora, roztomilá, do dětské pohádkové hry, ať sedí k herbářovému stylu.

```
top-down view of a cute cartoon honeybee, plump fuzzy body with black and
golden-amber stripes, small translucent wings spread, soft children's book
illustration, warm storybook style, centered, flat neutral background,
soft shadow
```

---

## 2) Pylová zrna — `pollen_*.png` (12×)

Styl pro všechna: **barevná fotografie ze skenovacího elektronového mikroskopu
(SEM), jedno zrno na střed, makro, plochý tmavý podklad.** Do každého promptu
vlož odpovídající **tvar** a **barvu** (níže). Kostra promptu:

```
false-color scanning electron microscope photograph of a single <TVAR> pollen
grain, <BARVA>, perfectly centered, extreme macro, sharp micro-texture,
scientific palynology, plain dark background, one grain only
```

Tvary (`<TVAR>` → anglicky) a mapování na soubory:

| Soubor | `<TVAR>` | `<BARVA>` | rostlina |
|---|---|---|---|
| `pollen_taraxacum.png` | echinate spiny sphere with many sharp spikes | orange | pampeliška |
| `pollen_helianthus.png` | densely echinate spiny sphere, long spikes | deep orange | slunečnice |
| `pollen_solidago.png` | small echinate spiny sphere | golden | zlatobýl |
| `pollen_salix.png` | reticulate netted ovoid, honeycomb surface | golden yellow | vrba |
| `pollen_papaver.png` | reticulate grain, fine netted surface | reddish | mák |
| `pollen_corylus.png` | smooth spheroid with three pores (triporate) | pale yellow | líska |
| `pollen_galanthus.png` | smooth ovoid, single furrow (monosulcate) | pale blue-white | sněženka |
| `pollen_trifolium.png` | tricolpate grain, rounded triangle, three furrows | violet purple | jetel |
| `pollen_hedera.png` | tricolporate grain, three furrows | olive tan | břečťan |
| `pollen_helleborus.png` | tricolpate grain, three furrows | pale green | čemeřice |
| `pollen_calluna.png` | tetrad of four fused pollen grains | pink | vřes |
| `pollen_pinus.png` | bisaccate pollen with two round air bladders (sacci) | cream | borovice |

Příklad (pampeliška):

```
false-color scanning electron microscope photograph of a single echinate spiny
sphere pollen grain with many sharp spikes, orange, perfectly centered, extreme
macro, sharp micro-texture, scientific palynology, plain dark background,
one grain only
```

---

## 3) Květy — `flower_*.png` (12×)

Jeden květ/květenství, **pohled shora nebo zpředu**, měkká pohádková ilustrace,
ať ladí se včelkou. Kostra:

```
single <ROSTLINA> flower, botanical storybook illustration, soft watercolor,
top view, centered, flat neutral background, no stem clutter
```

`<ROSTLINA>` (anglicky) → soubor:

| Soubor | `<ROSTLINA>` |
|---|---|
| `flower_taraxacum.png` | yellow dandelion bloom |
| `flower_helianthus.png` | sunflower head |
| `flower_papaver.png` | red field poppy |
| `flower_trifolium.png` | pink red clover flower head |
| `flower_salix.png` | yellow willow catkin |
| `flower_corylus.png` | hazel catkins |
| `flower_solidago.png` | goldenrod yellow plume |
| `flower_hedera.png` | greenish ivy flower umbel |
| `flower_calluna.png` | tiny pink heather bells |
| `flower_pinus.png` | pine male cone cluster |
| `flower_galanthus.png` | white nodding snowdrop |
| `flower_helleborus.png` | white five-petal hellebore |

---

## Tip na dávku

Zrna i květy jsou variace jedné šablony — v ComfyUI si nastav **jeden graf**
a jen měň text v pozitivním promptu (nebo použij node *Prompt from list* /
*Text Load Line From File* a nakrmit ho seznamem výše). Ať `SaveImage` ukládá
rovnou sem do `assets/` pod správným názvem.
