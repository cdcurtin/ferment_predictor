# Ferment Monitor

A single-page, client-side dashboard that predicts 10%, 50%, and zero-Brix
timing for many fermenting must samples at once, from a dropped spreadsheet
of readings. No server, no build step, no data leaves the browser — it's one
HTML file that loads the [SheetJS](https://sheetjs.com/) library from a CDN.

## Deploy to GitHub Pages

1. Create a new GitHub repo (or use an existing one).
2. Add `index.html` from this folder to the repo root (or to a `/docs` folder).
3. In the repo's **Settings → Pages**, set the source to the branch/folder you used.
4. Your dashboard is live at `https://<username>.github.io/<repo>/`.

That's it — no dependencies to install, nothing to build.

## Using it

1. Open the page and fill in `ferment_readings_template.xlsx` with your own
   readings (see the `instructions` sheet in that file), or drag in your own
   spreadsheet with columns `ferment_id`, `datetime`, `brix`.
2. Drop the file onto the page, or click **Choose file**.
3. Each ferment gets its own 4-parameter logistic fit; the table shows
   predicted 10% drop, 50% drop, and zero-Brix timing for every one, sorted
   so whichever ferment needs attention soonest is at the top.
4. Click a row to expand its full fitted curve with confidence bands.

Readings persist in your browser's local storage between visits (per
browser, per device — nothing is synced or uploaded).

## Model

Same model as the companion single-ferment predictor: a 4-parameter
logistic decay, `brix(t) = L + (U−L) / (1 + exp((t−t50)/s))`, fit per
ferment as a MAP estimate using priors drawn from 24 historical replicate
curves (U = 25.7 ± 0.6, L = −1.45 ± 0.32, t50 = 3.43 ± 0.27 d,
s = 1.18 ± 0.22, measurement noise ≈ 0.68 Brix). Zero-Brix timing is only
defined once a ferment's fitted plateau (L) is below zero.
