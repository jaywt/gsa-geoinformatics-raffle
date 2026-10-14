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

## Important
Data lives in the browser's localStorage on the selling device. Use the same browser
for the whole raffle and use **Backup (JSON)** daily. Backups contain participants'
contact details — do NOT commit them to GitHub.

## Publish via GitHub Pages
The repo can be public: it holds no participant data. Enable Pages (Settings → Pages → main branch).
