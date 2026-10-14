# GSA Geoinformatics & Data Science Division — Raffle Lucky Draw

A single-page, zero-dependency web app for running the division's raffle: **$1 per ticket**, **3 prizes**, **one draw per day**.

**🎟️ Live app: https://jaywen.com/gsa-geoinformatics-raffle/**

Built for the [GSA Geoinformatics & Data Science Division](https://community.geosociety.org/geoinformaticsdivision/home).
The division logo is used with reference to the division's page and remains GSA's property.

> **Important:** this is a static site. Ticket data is stored in the browser of the device used for selling, **not on a server**. Sell from one device and one browser only, and set up the backup folder (see [Protecting buyer data](#protecting-buyer-data)) before taking real sales.

## How the raffle works
- $1 per ticket, no cap per person. Chance of winning is proportional to the tickets in the pool.
- Each sale records a name and one contact (email or phone). The same contact counts as the same person, so repeat purchases add up.
- One draw per prize (3 total). The app warns if a draw was already made today.
- **Winners are removed from the pool** and can buy new tickets to re-enter later draws.
- **Non-winners stay in the pool.** Selling continues between draws.
- Winners are picked with the browser's cryptographically secure random generator (`crypto.getRandomValues`); the result is decided before the on-screen animation starts.

## Features
- Ticket sales form with input validation (name, email/phone, whole-number quantity)
- Live stats: revenue, tickets in pool, participants, draws done
- Current pool with each person's tickets and chance of winning
- Editable prize names, draw animation, winners table
- Sales log with the ability to void a sale that is still in the pool
- "Hide contacts" button for projecting the screen
- CSV export, JSON backup/restore, and a type-to-confirm full reset
- Status banner showing whether buyer data is safely backed up

## Quick start (event day)
1. Open the live app in **Chrome or Edge** on the selling device.
2. Click **Choose backup folder** in the top banner and pick a **private folder inside Dropbox** (e.g. `Dropbox/raffle-backup`).
3. Make a test sale and draw, confirm files appear in the folder, then use **Admin → Reset everything** (type `RESET`) to clear test data.
4. Sell tickets. Wait for the banner to be **green** before walking away from the device.
5. After each page reload, click the banner's **Re-approve folder access** button (a Chrome security rule).

## Protecting buyer data
Buyer data lives in the browser (localStorage) on the selling device, and is copied after every sale / void / draw to the **backup folder** you choose (Chrome or Edge):

| File | Contents |
|---|---|
| `raffle-latest.json` | Full state, overwritten on every change |
| `raffle-YYYY-MM-DD.json` | One snapshot per day (the last write of that day) |
| `raffle-sales-latest.csv` | Readable buyer list |

**Status banner**
- 🟢 Green — the latest change is saved in the backup folder.
- 🔴 Red — no folder chosen, folder access expired, a write failed, changes pending, **or** the folder holds *newer* data than the browser (e.g. browser storage was cleared). In that last case nothing is overwritten; use **Restore from folder**.

**Other safeguards**
- The app asks the browser to mark its storage as persistent.
- Safari/Firefox can't write to folders. There the app downloads a timestamped JSON after every change instead (set the browser's Downloads folder to a Dropbox folder). This fallback can be turned off in Admin.
- Manual **Backup (JSON)** / **Restore backup** and **Export sales CSV** are always available.

**Privacy:** backups contain participants' names and contact details. Keep the Dropbox folder private and never commit backups to GitHub (`.gitignore` blocks the usual filenames). The repository and the website contain **no participant data**.

## Limitations
- Single device: there is no shared server or login, and other people opening the link see their own empty copy.
- Anyone with access to the device/page can sell, void, draw and reset — there is no authentication.
- Data is tied to the browser profile and site address. Clearing site data, using private mode, or switching browsers/computers starts empty (restore from the backup folder).
- The folder-backup feature is Chrome/Edge only. Dropbox syncing is handled by Dropbox, not by the app.

## Run locally
```bash
git clone https://github.com/jaywt/gsa-geoinformatics-raffle.git
cd gsa-geoinformatics-raffle
python3 -m http.server 8000
# open http://localhost:8000
```
Opening `index.html` directly also works, but some browsers restrict folder access on `file://` pages, so prefer the local server.

## Deployment
Hosted with GitHub Pages from the `main` branch (root). Pushing to `main` redeploys automatically; no build step.

## Files
| File | Purpose |
|---|---|
| `index.html` | The entire app (HTML, CSS, JavaScript) |
| `logo.png` | Division logo from the GSA community page |
| `.gitignore` | Keeps backup and CSV exports out of git |
