# GSA Geoinformatics & Data Science Division — Raffle Lucky Draw

Single-page, no-dependency web app for the division raffle ($1/ticket, 3 prizes, one draw per day).

## Run locally
    python3 -m http.server 8000
    # open http://localhost:8000

(Opening `index.html` directly also works.)

## Rules implemented
- $1 per ticket, no cap per person; chance is proportional to tickets in the pool.
- Each sale records name + one contact (email or phone). Same contact = same person.
- Winners are removed from the pool; they may buy new tickets to re-enter.
- Non-winners stay in the pool. Selling continues between draws.

## Protecting buyer data
Buyer data lives in the browser (localStorage) on the selling device, and is copied after
every sale / void / draw to a **backup folder** you choose (Chrome or Edge):

- `raffle-latest.json` — full state, overwritten each time
- `raffle-YYYY-MM-DD.json` — one snapshot per day (last write of that day)
- `raffle-sales-latest.csv` — readable buyer list

Setup: click **Choose backup folder** in the banner and pick a folder inside Dropbox.
After a page reload Chrome asks you to re-approve folder access (one click on the banner).

The banner at the top is green only when the latest change is safely in the folder; it turns
red if there is no folder, access expired, a write failed, or the folder holds *newer* data than
the browser (e.g. browser storage was cleared) — in that case nothing is overwritten and you can
**Restore from folder**.

Safari/Firefox can't write to folders; the app then downloads a timestamped JSON after every
change (set the browser's Downloads folder to a Dropbox folder).

Backups contain participants' contact details — keep the Dropbox folder private and do NOT
commit them to GitHub (`.gitignore` blocks the usual filenames).

## Publish via GitHub Pages
The repo can be public: it holds no participant data. Enable Pages (Settings → Pages → main branch).
