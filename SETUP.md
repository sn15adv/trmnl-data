# How to set this up on GitHub

This package contains 4 files that go into a GitHub repo. The repo will
automatically keep `news.json` fresh every 15 minutes, and your TRMNL plugin
polls the raw URL.

## Files in this package

```
trmnl-data/
├── README.md                              ← description (optional but nice)
├── news.json                              ← placeholder, auto-overwritten
├── hindu-festivals.json                   ← your festivals (you can move from gist)
└── .github/
    └── workflows/
        └── update-news.yml                ← the auto-update action
```

## Step 1 — Create the repo

1. Go to https://github.com/new
2. Repository name: **`trmnl-data`** (any name works, but match what you put in TRMNL)
3. **Public** (required — TRMNL needs to fetch the raw file without auth)
4. Tick **"Add a README file"**
5. Click **Create repository**

## Step 2 — Upload the files

Easiest method: drag-and-drop the whole `trmnl-data` folder.

1. On your new repo's main page, click **Add file → Upload files**
2. Drag the **contents** of the `trmnl-data` folder (not the folder itself)
   into the upload area: `news.json`, `hindu-festivals.json`, `README.md`,
   and the `.github` folder
3. Scroll down → commit message: "Initial setup" → **Commit changes**

If drag-and-drop doesn't pick up the `.github` folder (sometimes browsers
hide hidden folders), do this for the workflow file instead:

1. Click **Add file → Create new file**
2. In the filename box, type: `.github/workflows/update-news.yml`
   (the slashes auto-create the folders)
3. Paste the content of `update-news.yml`
4. Commit

## Step 3 — Make sure Actions can write to the repo

GitHub locks down workflow permissions by default. Open them up:

1. In your repo, click **Settings** (top tab)
2. Left sidebar: **Actions → General**
3. Scroll to **Workflow permissions**
4. Select **"Read and write permissions"**
5. Click **Save**

Without this, the action will run but fail at the "git push" step.

## Step 4 — Run the action manually for the first time

1. Click the **Actions** tab in your repo
2. If prompted "Workflows aren't being run on this repository" → click
   **"I understand my workflows, go ahead and enable them"**
3. In the left sidebar, click **Update news**
4. On the right, click **Run workflow** → green **Run workflow** button
5. Wait ~30 seconds, refresh the page
6. The run should show a green checkmark ✅
7. Go back to your repo's main page → click `news.json` → you should see
   5 real BBC headlines

If the run shows red ❌, click into it to see the error. 90% of the time
it's the permissions thing in Step 3.

## Step 5 — Update TRMNL polling URLs

In your private plugin's Polling URL list, find the saurav.tech BBC line
and replace it with:

```
https://raw.githubusercontent.com/YOUR_USERNAME/trmnl-data/main/news.json
```

Replace `YOUR_USERNAME` with your actual GitHub username (and `trmnl-data`
with your repo name if you used a different one).

If you want to also move your festivals file into this repo (cleaner —
one place for everything), update that URL too:

```
https://raw.githubusercontent.com/YOUR_USERNAME/trmnl-data/main/hindu-festivals.json
```

Save → Force Refresh.

## Step 6 — The markup is unchanged

Your existing markup already reads `IDX_9.articles[].title` — that's the
exact shape this `news.json` produces. No markup edit needed.

## What happens after this

- Every 15 minutes, GitHub runs the action automatically.
- It fetches BBC's RSS feed, extracts 5 titles, and updates `news.json`.
- The raw URL serves the latest version.
- TRMNL polls that URL on its own 15-minute cycle and renders fresh news.

You'll see a small green checkmark on every commit in your repo from
"github-actions[bot]". That's the normal sign things are working.

## If you want to switch news sources later

Edit `.github/workflows/update-news.yml`, find the line:

```python
url = "https://feeds.bbci.co.uk/news/world/rss.xml"
```

Change to any RSS feed:

- BBC UK: `https://feeds.bbci.co.uk/news/uk/rss.xml`
- BBC Tech: `https://feeds.bbci.co.uk/news/technology/rss.xml`
- NYT homepage: `https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml`
- Guardian world: `https://www.theguardian.com/world/rss`
- Reuters: `https://feeds.reuters.com/reuters/topNews`

Commit the change. Next scheduled run picks it up automatically.
