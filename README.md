# Mumm — seafront hotel website

Static site for **Mumm**, a small seafront hotel on Passeig Marítim, 88, Vilanova i la Geltrú.

Three languages, server-rendered HTML per locale, no framework and no runtime dependencies.

## Live site

- Catalan (default): https://mummhotel.vilanovailageltru.info/
- Spanish: https://mummhotel.vilanovailageltru.info/es/
- English: https://mummhotel.vilanovailageltru.info/en/

## Page layout

Photo-led, because for a seafront hotel the view is the product: a full-bleed photo stage
with the booking action on it, a three-item facts band, room types with their amenities,
a short honest description of what the hotel is and is not, guest reviews, and a booking
section with a consent-gated map.

The rating is shown as a plain score without a star badge, because the average is below 4.
The review count is displayed because it exceeds 50.

## Structure

```
index.html                 Catalan (default locale)
es/index.html              Spanish
en/index.html              English
ca/index.html              redirect to /
src/template.html          single template used by the build
src/data.json              hotel data (address, phone, coordinates, rating)
src/locales.json           locale list, default, hreflang codes
src/i18n/{ca,es,en}.json   all copy, per locale
scripts/build.mjs          emits the locale pages + sitemap, robots, manifest
scripts/assets.mjs         app icons and Open Graph image
scripts/card.mjs           handover card (10.5 x 14.5 cm @ 300 dpi)
scripts/verify.mjs         headless verification of the built output
assets/css/styles.css      all styles, responsive from 320 px up
assets/js/main.js          navigation, reveal, cookie consent, lazy map
assets/fonts/              self-hosted Fraunces and Inter (woff2 subsets)
assets/photos/handover.png handover card with a QR to the live site
```

## Build

```bash
node scripts/build.mjs
node scripts/assets.mjs
node scripts/card.mjs
node scripts/verify.mjs
```

`build.mjs` fails on unresolved template placeholders, empty `href`/`src`, or invalid JSON-LD.

## Deploy

Pushing to `main` runs `.github/workflows/deploy-aws.yml`: it builds, authenticates to AWS
with OIDC (no stored keys), and uploads to
`s3://dr-static-websites/vilanovailageltru.info/mummhotel/`. Served from the bucket CDN.

## Technical notes

- **SEO**: per-locale title, description, canonical, reciprocal hreflang with `x-default`,
  Open Graph and Twitter cards, sitemap with alternates, robots.txt, and `Hotel` structured
  data. Every absolute URL derives from the site's own domain.
- **Accessibility**: single `h1`, skip link, visible focus rings, body text at or above 4.5:1
  contrast, `aria-expanded` toggle, Escape closes the drawer.
- **Privacy**: the cookie notice has accept and reject. Nothing is fetched from Google —
  analytics or map — until the visitor accepts; rejecting loads neither and is remembered.
- **Honesty**: the average rating and its count are published as they are, and the reviews
  shown are positive extracts that link to the full set on Google.
