# trmnl-data

Static JSON files served as raw URLs to my TRMNL plugin.

## Files

- **`news.json`** — Latest 5 BBC World headlines, auto-updated every 15 minutes
  by the GitHub Action in `.github/workflows/update-news.yml`.
- **`hindu-festivals.json`** — Manually maintained list of upcoming Hindu
  festivals. Update once a year.

## Raw URLs (for TRMNL Polling)

```
https://raw.githubusercontent.com/YOUR_USERNAME/trmnl-data/main/news.json
https://raw.githubusercontent.com/YOUR_USERNAME/trmnl-data/main/hindu-festivals.json
```

## Setup

1. Make this repo **public** (GitHub raw URLs require it for unauthenticated fetch).
2. Go to **Actions** tab → enable workflows if prompted.
3. Click **Update news** → **Run workflow** to populate `news.json` immediately.
4. After that, GitHub schedules it every 15 minutes automatically.

## Troubleshooting

- **`news.json` still says "Loading…"** — the action hasn't run yet. Trigger
  it manually from the Actions tab.
- **Action fails with permissions error** — Settings → Actions → General →
  Workflow permissions → set to **Read and write permissions** → Save.
- **Schedule is delayed** — GitHub cron schedules can lag by 5–15 minutes
  during high load. Normal for the free tier.
- **Want a different news source?** — Edit the `url` line in
  `update-news.yml`. Any RSS feed with standard `<item><title>...</title></item>`
  structure will work (NYT, Reuters, Guardian, etc.).
