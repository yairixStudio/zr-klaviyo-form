# zr-klaviyo-form

A standalone, multilingual newsletter signup form for **Zielinski & Rozen** that submits directly to
Klaviyo's [Client Subscriptions API](https://developers.klaviyo.com/en/reference/create_client_subscription).
No build step, no dependencies — static HTML hosted on GitHub Pages. It can be shared as a link,
printed as a QR code, or embedded as an `<iframe>` on any website.

**Live:** https://yairixstudio.github.io/zr-klaviyo-form/

## Pages

| Page | Path | Purpose |
|---|---|---|
| **Form** | `/` | The live signup form visitors fill in |
| **QR generator** | `/generate/` | Build a campaign link and download a QR code (SVG/PNG) for print, ads, in-store |
| **Guide** | `/guide/` | How it works, URL parameters, and ready-to-paste iframe embed code |

## How it works

- POSTs to `https://a.klaviyo.com/client/subscriptions/`. Auth is the public company ID only
  (no secret key), and the endpoint is CORS-enabled, so it works from any origin including iframes.
- Consent links to the GDPR privacy policy on the main site.

## Languages

11 languages with a switcher at the bottom of the form: English, Arabic, Chinese, French, German,
Hebrew, Hindi, Italian, Portuguese, Russian, Spanish. Country names localize automatically via
`Intl.DisplayNames`. The starting language is auto-selected (`?lang=` → saved → browser → country →
IP/timezone) and falls back to English.

## URL parameters

| Param | Use |
|---|---|
| `city` | Prefills the City field |
| `country` | Prefills Country (ISO code `FR` or name `France`) |
| `channel` / `source` | Marketing channel (default `qr-code`) |
| `store_id` / `store` | Originating store / location |
| `qr_id` / `qr` / `code` | Specific QR / campaign code |
| `lang` | Force a starting language (`fr`, `es`, `ar`, …) |
| `utm_*` | Stored as-is |
| `debug=1` | Shows the outgoing payload |

Example: `…/?store_id=paris-marais&city=Paris&country=FR&channel=qrcode`

## Embedding

Get the snippet from `/guide/`, or use the dynamic version that forwards the host page's params and
auto-resizes the iframe:

```html
<iframe id="zr-form"
  style="width:100%;max-width:640px;height:560px;border:0;display:block;margin:0 auto;"
  title="Sign up — Zielinski & Rozen" loading="lazy"></iframe>

<script>
  (function () {
    var base = 'https://yairixstudio.github.io/zr-klaviyo-form/';
    var iframe = document.getElementById('zr-form');
    iframe.src = base + window.location.search;        // forward the page's URL params
    window.addEventListener('message', function (e) {
      if (e.data && e.data.type === 'zr-klaviyo-form:height') {
        iframe.style.height = e.data.height + 'px';     // auto-resize
      }
    });
  })();
</script>
```

**Shopify:** paste into a *Custom Liquid* block (the rich-text editor strips `<script>` and iframes).
