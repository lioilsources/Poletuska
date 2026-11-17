# Plán: generování assetů Poletušky přes Claude Code na M2

**Klíčový vhled:** Claude Code (běžící lokálně na tvém M2) sám obrázky
nevygeneruje, ale **umí ovládat ComfyUI přes jeho HTTP API**. Diffusion točí
GPU tvého M2, Claude orchestruje celou dávku, pojmenování a průhlednost.
Výsledek: všech 25 assetů se nasype do `assets/` a hra si je automaticky vezme.

Stroj: **Mac Mini M2** — Apple M2, 8 CPU (4+4) + 10-core GPU, 16 GB unified,
macOS 26.5 (arm64).

---

## Dělba práce

| Tvůj M2 | Claude Code |
|---|---|
| instalace ComfyUI + stažení modelu | driver skript pro ComfyUI API |
| GPU diffusion (Metal / MPS) | šablonování promptů, seedy |
| — | ořez na čtverec, průhlednost (rembg), pojmenování |
| — | zapojení do hry + iterace na vadných kusech |

---

## Fáze 0 — jednorázová příprava (ty, ~30 min)

ComfyUI a model musíš nainstalovat ty (stahuje gigabajty a potřebuje GPU).

```bash
# ComfyUI
git clone https://github.com/comfyanonymous/ComfyUI ~/ComfyUI
cd ~/ComfyUI
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt

# SDXL checkpoint -> ~/ComfyUI/models/checkpoints/
#   (např. Juggernaut XL nebo DreamShaper XL z Hugging Face / Civitai)

# spuštění serveru — API běží na http://127.0.0.1:8188
python main.py --lowvram
```

Průhlednost řeší Claude přes `rembg` (běží na CPU, žádný extra Comfy node):

```bash
pip install rembg onnxruntime pillow
```

### Poznámka k výkonu (16 GB M2)
- SDXL (1024²) poběží na MPS, ale **paměťově napnutě** → `--lowvram`, zavři
  těžké appky. Čas na obrázek: jednotky až nižší desítky sekund.
- Kdyby SDXL funěl, **fallback SD 1.5** (512²) + upscale — svižné.
- 25 assetů je jednorázová dávka po jednom, ne realtime — zvládne se.

---

## Fáze 1 — orchestrace (Claude Code, přes Bash)

Až poběží ComfyUI, Claude napíše `assets/generate.py`, který:

1. sestaví ComfyUI API graf (txt2img, SDXL) — checkpoint a rozlišení
   nastavené na jednom místě nahoře ve skriptu,
2. projde 25 promptů (tvary a barvy pylů už namapované v `assets/PROMPTS.md`
   a `assets/manifest.json`),
3. každý pošle `POST /prompt`, poolí `GET /history/<id>`, stáhne PNG přes
   `GET /view`,
4. ořízne na čtverec, odmázne pozadí přes `rembg` a uloží pod správným
   názvem do `assets/` (`bee.png`, `pollen_taraxacum.png`, `flower_papaver.png` …).

Spuštění (po startu ComfyUI):

```bash
cd $DEV_ROOT/VceliRise
python3 assets/generate.py            # celá dávka
python3 assets/generate.py taraxacum  # jen vybrané (re-roll)
```

---

## Fáze 2 — iterace

Hra assety načítá sama (chybějící soubor = fallback na vektorovou kresbu),
takže:

1. otevři `poletuska.html`, reload,
2. co se nepovede, **přegeneruje se jen to** — jiný seed / doladěný prompt,
3. ladíme po kusech, dokud nesedí.

---

## Seznam assetů (25)

Plné znění promptů: **`assets/PROMPTS.md`**. Formát: čtvercové PNG, průhledné
pozadí, 1024×1024, motiv na střed.

- `bee.png` — včelka shora
- `pollen_*.png` (12) — pylová zrna, SEM styl, dle reálné morfologie
  (echinate / reticulate / triporate / tricolpate / tetrad / saccate)
- `flower_*.png` (12) — květy, měkká pohádková ilustrace

---

## Otevřené

- **„SPSRK“** — netuším, jestli je to konkrétní checkpoint/workflow; když
  pošleš jméno, prompty i graf ve `generate.py` doladím přímo pod něj.
- Verzování PNG v gitu: buď je commitnout, nebo dát `.gitignore` na
  `assets/*.png` a nechat je jen lokálně — na rozhodnutí.

---

## Další krok

Claude na požádání rovnou napíše `assets/generate.py`, ať je připravený a po
instalaci ComfyUI se jen spustí.
