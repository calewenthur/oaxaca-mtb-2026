# Oaxaca MTB Trip Tracker

Single-file, self-contained HTML site for the Oaxaca MTB trip (Oct 8-18, 2026). No build step, no dependencies -- just `index.html`.

- **Live site:** https://calewenthur.github.io/oaxaca-mtb-2026/
- **GitHub repo:** https://github.com/calewenthur/oaxaca-mtb-2026
- **Hosting:** GitHub Pages, served from the `main` branch root. Any push to `main` triggers an automatic rebuild (usually live within ~30-60 seconds).

## Moving this into Claude Code

This folder is already a git repo with `origin` pointing at the GitHub repo above and one commit checked in. To pick up editing here with Claude Code instead of the browser:

1. Copy this whole folder to wherever you keep projects, e.g. `~/code/oaxaca-mtb-2026`.
2. Open a terminal in that folder and authenticate to GitHub if you haven't already on this machine:
   - Easiest: `gh auth login` (GitHub CLI), then things below just work, or
   - set up an SSH key / personal access token for `git push` over HTTPS.
3. The local history here doesn't share commits with the real GitHub history yet, so the first push needs to reconcile the two. Simplest path:
   ```bash
   git fetch origin
   git reset --soft origin/main   # keep your working file, adopt GitHub's history
   git add -A
   git commit -m "sync from Cowork" --allow-empty
   git push origin main
   ```
   After that, `git pull` / `git push` behave normally for all future edits.

## Making an update (once set up)

```bash
# edit index.html directly
git add index.html
git commit -m "describe the change"
git push
```

GitHub Pages rebuilds automatically -- check progress at
https://github.com/calewenthur/oaxaca-mtb-2026/actions
and refresh the live site after the run finishes.

## What's in index.html

Everything is inline in one file: CSS in a `CSS_TEXT` template literal injected into a `<style>` tag, and the page markup built by a small `render()` function driven by a few data structures near the top of the `<script>` block:

- `DAYS` -- the day-by-day itinerary (Oct 8-18). Each entry is `{d, n, day, badge, label, loc, title, desc}`.
- `GUIDE_MXN`, `FX_RATE`, `LODGING_USD` -- the constants behind the cost splits at the top of the page.
- `costGridHTML()` / `paymentNotesHTML()` -- the Costs and Payments sections, including the Airbnb listing link.
- `flightsHTML()` / `gearHTML()` -- Flights and What to Bring sections.
- `calendarHTML()` / `dayICSHref()` -- the Day by Day section and its "+ Add to Calendar" `.ics` download links.

To add a dinner reservation, adjust a cost, or fix a date, search for the relevant text directly in `index.html` -- everything is plain strings, no templating engine.
