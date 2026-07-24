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
  of `[set point, adjustment]`, highest first. Positive numbers add to the multiple,
  negative subtract. Add or remove set points freely; more points shapes the curve
  more precisely.
- `floor` / `cap` — the multiple is clamped to this range. If the adjustments push
  past a limit, the page says so rather than showing a factor list that doesn't add up.
- `rangeSpread` — the ± band around the point estimate.

Two behaviors worth knowing, because they are what make the output defensible:

- **Adjustments blend between set points.** A firm at 87% recurring lands between the
  80% and 90% marks. Nothing jumps at a threshold, so nudging a slider one notch never
  swings the valuation by a million dollars.
- **A blank field scores neutral, never a penalty.** Leaving out household count simply
  drops that driver to 0.00 and says so on the page, instead of quietly scoring the firm
  as if it had the smallest clients possible.

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
