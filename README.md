# Nano Banana 2 Lite API (`gemini-3.1-flash-lite-image-ext`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0125 per image** — un único endpoint compatible con OpenAI en `https://api.apimart.ai/v1`, sin plan mensual. *(observado el 2026-10-12)*

**[Obtener una clave de API](https://go.apimart.ai/k-6b0333)** · **[Precios en vivo](https://go.apimart.ai/k-68a190)** · **[Página del modelo](https://go.apimart.ai/k-31ecc7)** · [⚡ Inicio rápido en 60 segundos](#quickstart)

**Por qué los equipos usan Nano Banana 2 Lite (`gemini-3.1-flash-lite-image-ext`) a través de APIMart**

- **Una clave, todo el catálogo.** La misma base URL `https://api.apimart.ai/v1` y la misma cabecera `Authorization` dan acceso a Nano Banana 2 Lite (`gemini-3.1-flash-lite-image-ext`) y a más de 300 modelos de imagen, vídeo y lenguaje: cambia el campo `model`, no tu cliente.
- **Mínimo de $1, pago por uso.** Sin suscripción ni plan prepago que dimensionar por adelantado: recarga desde $1 y úsalo en llamadas.
- **El cargo vuelve en la respuesta.** Cada tarea completada informa del importe facturado (`cost` / `credits_cost`); una tarea marcada como `failed` se reembolsa.
- **Asíncrono por diseño.** Envía, toma el `task_id`, consulta `GET /v1/tasks/{id}`: el batching y los reintentos son trabajo de cola normal, no una integración a medida.

<!-- /conv-kit:v1 -->



Nano Banana 2 Lite is the cheaper Gemini 3.1 Flash Lite Image route, priced per image.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `gemini-3.1-flash-lite-image-ext` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-31ecc7) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.015625 | 0.0125 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-68a190) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-31ecc7) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"gemini-3.1-flash-lite-image-ext","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "gemini-3.1-flash-lite-image-ext", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**¿Cuál es el id del modelo Nano Banana 2 Lite?**

`gemini-3.1-flash-lite-image-ext`. Consulta la tabla de rutas de arriba.

**¿Cómo se factura?**

**from $0.0125 per image**. La tarea completada informa del importe exacto en su campo `cost`; una tarea fallida se reembolsa.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-6b0333) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [nano-banana-2-lite-prompts-es](https://github.com/chz6q2o7d/nano-banana-2-lite-prompts-es)
- **Same model, other languages** — 9 more language builds: 中文 [nano-banana-2-lite-api-cn](https://github.com/iur08dy-c157/nano-banana-2-lite-api-cn) · Deutsch [nano-banana-2-lite-api-de](https://github.com/k3idrzqs5wbuf/nano-banana-2-lite-api-de) · Français [nano-banana-2-lite-api-fr](https://github.com/fz350t/nano-banana-2-lite-api-fr) · Relay/gateway [nano-banana-2-lite-api-gateway](https://github.com/tjk4cf/nano-banana-2-lite-api-gateway) · Bahasa [nano-banana-2-lite-api-id](https://github.com/dz2oqx/nano-banana-2-lite-api-id) · 日本語 [nano-banana-2-lite-api-ja](https://github.com/yv-kf9hls0kr/nano-banana-2-lite-api-ja) · 한국어 [nano-banana-2-lite-api-ko](https://github.com/rguc5r0zdtlm/nano-banana-2-lite-api-ko) · Português [nano-banana-2-lite-api-pt](https://github.com/mxtkvc6u/nano-banana-2-lite-api-pt) · Русский [nano-banana-2-lite-api-ru](https://github.com/nkdje/nano-banana-2-lite-api-ru)
- **More models in this language** — [midjourney-api-es](https://github.com/xbuu0/midjourney-api-es) · [gemini-2-5-flash-image-preview-api-es](https://github.com/ekw3w/gemini-2-5-flash-image-preview-api-es) · [nano-banana-api-es](https://github.com/gbm5mfl1/nano-banana-api-es) · [gemini-3-1-flash-image-preview-api-es](https://github.com/x3kcczm8l29xn/gemini-3-1-flash-image-preview-api-es) · [nano-banana-2-api-es](https://github.com/d63hy9t-4pp/nano-banana-2-api-es) · [gemini-3-pro-image-preview-api-es](https://github.com/uv9d8/gemini-3-pro-image-preview-api-es)
- **Topic hubs** — every build of this model, all languages: [nano-banana-2-lite](https://github.com/topics/nano-banana-2-lite) · [nano-banana-2-lite-api](https://github.com/topics/nano-banana-2-lite-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-31ecc7> | `apimart.ai/es/model/nano-banana-2-lite` |
| Live pricing | <https://go.apimart.ai/k-68a190> | `apimart.ai/es/pricing` |
| Get an API key | <https://go.apimart.ai/k-6b0333> | `apimart.ai/es/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Nano Banana 2 Lite is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
