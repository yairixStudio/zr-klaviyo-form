# zr-klaviyo-form

A standalone, minimalist signup form for **Zielinski & Rozen** that submits directly to
Klaviyo's [Client Subscriptions API](https://developers.klaviyo.com/en/reference/create_client_subscription).
Built as a single static `index.html` (no build step, no dependencies) and designed to be
embedded as an `<iframe>` on any website.

**Live:** https://yairixstudio.github.io/zr-klaviyo-form/

## How it works

- POSTs to `https://a.klaviyo.com/client/subscriptions/?company_id=RNG3Ym` (list `XfZuXJ`).
- Auth is the public `company_id` only — no secret key, safe to ship client-side.
- The endpoint is CORS-enabled, so it works from any origin, including inside an iframe.

## URL parameters

The form reads these from **its own** URL (the iframe `src`), pre-fills fields, and attaches
them as profile properties:

| Param | Use |
|---|---|
| `city` | Pre-fills the City field |
| `country` | Pre-fills Country (accepts ISO code `FR` or name `France`) |
| `channel` / `source` | Stored as `channel` property (default `qr-code`) |
| `store_id` / `store` | Stored as `store_id` |
| `qr_id` / `qr` / `code` | Stored as `qr_id` |
| `utm_*` | Stored as-is |
| `debug=1` | Shows the debug panel with the outgoing payload |

Example: `…/?city=paris&country=france&channel=qrcode`

## Embedding

```html
<iframe
  src="https://yairixstudio.github.io/zr-klaviyo-form/?city=paris&country=france&channel=qrcode"
  style="width:100%;max-width:680px;height:560px;border:0;"
  title="Sign up — Zielinski & Rozen"
  loading="lazy"></iframe>
```

### Optional auto-resize

The page posts its height to the parent via `postMessage`. To auto-size the iframe:

```html
<script>
  window.addEventListener('message', function (e) {
    if (e.data && e.data.type === 'zr-klaviyo-form:height') {
      document.getElementById('zr-form').style.height = e.data.height + 'px';
    }
  });
</script>
```

(Give the iframe `id="zr-form"`.) See `embed-example.html` for a full working demo.
