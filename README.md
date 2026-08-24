# The One-Person Startup With AI — Sales Funnel

Two pages:

- `index.dc.html` — long-form sales page
- `thank-you.dc.html` — post-payment celebration + download page

Both are single self-contained files (no build step, no framework, Google Fonts only). Open either directly in a browser.

---

## 1. Where to paste your payment URL

Open `index.dc.html` and find, near the top of the `<script data-dc-script>` block:

```js
const PAYMENT_URL = "PASTE_PAYMENT_GATEWAY_URL_HERE";
```

Replace the string with your checkout link (Razorpay payment page, Stripe Payment Link, Gumroad product URL, etc.).

Every CTA on the page — header, hero, pricing card, final CTA, sticky bar — uses this one value.

You can also set it without touching code: open the page in the editor and use the **Tweaks** panel → *Checkout → Payment URL*. The tweak wins over `PAYMENT_URL` when set.

## 2. Where to paste your eBook download URL

Open `thank-you.dc.html`:

```js
const DOWNLOAD_URL = "PASTE_EBOOK_DOWNLOAD_URL_HERE";
const SHARE_URL    = "https://example.com/";
```

- `DOWNLOAD_URL` — a direct link to the PDF (S3, Cloudflare R2, Google Drive direct link, Gumroad file URL…).
- `SHARE_URL` — your live sales page URL, used by the LinkedIn / X / Copy-link buttons.

Also available in the Tweaks panel → *Delivery*.

## 3. How the payment redirect works

Every buy button is `<a id="buy-now-btn" href="#">` with a JS click handler. The handler:

1. prevents the default anchor jump,
2. plays a ripple micro-interaction,
3. redirects the top-level window to `PAYMENT_URL`.

If the URL is still the `PASTE_…` placeholder, the button shows an alert instead of navigating — so you cannot accidentally ship a dead checkout.

No gateway SDK is embedded. Swapping providers is a one-line change.

## 4. Hosting on Netlify or Vercel

**Netlify** — drag the project folder onto app.netlify.com/drop, or:

```
netlify deploy --prod --dir .
```

**Vercel** —

```
vercel --prod
```

Both serve static files as-is. If you want clean URLs, rename `index.dc.html` → `index.html` and `thank-you.dc.html` → `thank-you.html`, and update the two cross-links (footer link on the sales page, "Back to the book page" on the thank-you page).

Before going live, replace the placeholders in `<helmet>`: the `canonical` URL, the Open Graph / Twitter `og:image` path, and the price/currency in the Product schema.

## 5. Connecting the success redirect

**Razorpay** — in Payment Pages / Payment Links, set the *Success / Callback URL* to `https://yourdomain.com/thank-you.html`. For Payment Links via API, pass `callback_url` and `callback_method: "get"`.

**Stripe** — Payment Links: *After payment → Don't show confirmation page → Redirect to `https://yourdomain.com/thank-you.html`*. Checkout Sessions: set `success_url`.

**Gumroad** — Product → Content/Settings → *Redirect customers to a URL after purchase*.

Security note: the thank-you page is a public URL, so treat `DOWNLOAD_URL` as semi-public. For real protection use a signed/expiring link (S3 presigned URL, Gumroad's own delivery) rather than a permanent public file, and set the page to `noindex` (already done).

---

## Copy accuracy

All numbers on the page come from the book itself — 114 pages, 15 chapters, 4 appendices, 20 numbered prompts (A1–A20), 5 business models, a 13-week 90-day plan, and the six founder case studies from Chapter 12. No testimonials, customer counts, or income claims were invented. Prices (₹499 / ₹1,999) and the bonus "value" figures are placeholders — change them to your real numbers.

Placeholders left deliberately: refund policy (pricing card + FAQ), newsletter link (thank-you page), canonical URL, OG image.
