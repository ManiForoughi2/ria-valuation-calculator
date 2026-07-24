# RIA Firm Valuation Calculator

A lightweight, single-page valuation calculator for RIA firms, in the spirit of the
Carson valuation tool but rebuilt as a Booth & Company product. A principal enters a
few numbers and gets an indicative firm value, the multiple behind it, the factors
moving it, and an EBITDA cross-check.

It is one file (`index.html`), no build step, no dependencies, nothing sent anywhere.
Open it in a browser or host it as a static page. Booth branding (logo, favicon,
share preview) and a "Get a real valuation" lead form are built in.

## Run it locally

Double-click `index.html`, or serve the folder:

```
python3 -m http.server 8080
# then open http://localhost:8080
```

## Tune the methodology

The entire model lives in the `CONFIG` block near the bottom of `index.html`. Change
the numbers and the page updates, no other edits needed.

- `baseMultiple` — the starting revenue multiple (2.7×).
- `recurring`, `clientSize`, `fee`, `scale` — four adjustment tables. Each is a list
  of `[threshold, adjustment]` read top to bottom, first match wins. Positive numbers
  add to the multiple, negative subtract.
- `floor` / `cap` — the multiple is clamped to this range.
- `rangeSpread` — the ± band around the point estimate.

This is where Booth's own valuation analytics plug in.

## Collect leads (optional)

The "Get a real valuation" form works out of the box (it shows a thank-you). To
actually receive submissions, open the `LEAD` block just below `CONFIG` and set one:

- `endpoint` — a form URL (Google Form `formResponse`, Formspree, or your own handler).
- `email` — an address; submitting opens the visitor's email app pre-filled.

Leave both blank and nothing is sent anywhere.

## Save as PDF

The results card has a **Save as PDF** button (print stylesheet included), so a
principal can keep or share the estimate. Inputs, results, and methodology print
clean; the lead form and footer are hidden.

## Inputs

- Assets under management
- Annual gross revenue
- Number of client households
- Advisory (recurring) asset share

Derived automatically: average client size (AUM ÷ households) and average realized fee
(revenue ÷ AUM).

## Deploy

The folder is a static site. It works on GitHub Pages, Cloudflare Pages, Netlify, or
any static host, no configuration. For Cloudflare Pages, connect this repo and set the
build command to none and the output directory to the repo root.
