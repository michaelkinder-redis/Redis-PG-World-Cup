# ⚽ PG World Cup Q2 2026 — Scoreboard

Live leaderboard for the Redis Pipeline Generation World Cup.  
**Competition runs: 11 Jun – 19 Jul 2026**

---

## File structure

```
pg-world-cup/
├── index.html   ← The scoreboard page (don't edit this)
├── data.json    ← All team scores — edit this each week
└── README.md    ← This file
```

**The only file you ever need to touch is `data.json`.**

---

## Part 1 — First-time deployment (~20 minutes)

### Step 1: Create a GitHub repository

1. Go to [github.com](https://github.com) and sign in.
2. Click the **+** icon (top right) → **New repository**.
3. Name it `pg-world-cup` (or anything you like).
4. Set it to **Public** (Cloudflare Pages works with private repos too, but Public is simplest).
5. Click **Create repository**.
6. On the next screen, click **uploading an existing file**.
7. Drag and drop all three files (`index.html`, `data.json`, `README.md`) into the upload area.
8. Click **Commit changes**.

---

### Step 2: Deploy to Cloudflare Pages

1. Go to [cloudflare.com](https://cloudflare.com) and create a free account (or sign in).
2. From the dashboard, click **Workers & Pages** in the left sidebar.
3. Click **Create** → **Pages** → **Connect to Git**.
4. Authorise Cloudflare to access your GitHub, then select the `pg-world-cup` repo.
5. On the build settings screen:
   - **Framework preset:** None
   - **Build command:** *(leave blank)*
   - **Build output directory:** *(leave blank)*
6. Click **Save and Deploy**.

Cloudflare will deploy the site in about 30 seconds. You'll get a URL like:

```
https://pg-world-cup.pages.dev
```

That is your permanent scoreboard URL. It never changes.

---

## Part 2 — Updating scores each week (~5 minutes per update)

### The workflow

1. Pull new numbers from Backstory.com for all teams.
2. Share the updated spreadsheet with Claude (as you've been doing).
3. Claude generates a new `data.json` for you.
4. You replace the file in GitHub — see below.

### How to replace data.json in GitHub

1. Go to your `pg-world-cup` repository on GitHub.
2. Click on `data.json` in the file list.
3. Click the **pencil icon** (Edit this file) in the top right.
4. Select all the text (`Ctrl+A` / `Cmd+A`) and delete it.
5. Paste in the new `data.json` content.
6. Click **Commit changes** → **Commit changes** again.

Cloudflare will automatically redeploy within about 60 seconds. The scoreboard URL stays the same.

> **Tip:** You can also drag-and-drop replace the file. On the repo's main page, just drag the new `data.json` onto the page and GitHub will offer to replace it.

---

## data.json format reference

```json
{
  "lastUpdated": "2026-06-18",
  "aeTeams": [
    {
      "name":    "The Keyspaces",
      "manager": "Cameron Dickson",
      "region":  "AMER",
      "size":    6,
      "m1":      4,
      "m2":      2,
      "bonus":   0
    }
  ],
  "aeInd": [
    { "name": "Alice Smith", "m1": 3, "m2": 1, "bonus": 0 }
  ],
  "sdrTeams": [ ... ],
  "sdrInd":   [ ... ]
}
```

| Field    | Meaning                              |
|----------|--------------------------------------|
| `m1`     | AE: Discoveries completed / SDR: SALs |
| `m2`     | AE: NBMs completed / SDR: SAOs        |
| `bonus`  | AI PG bonus points (+2 per qualifying #pg-messaging post) |
| `size`   | Team headcount (used to normalise the team score) |

Scores are calculated automatically by the page — you only ever enter raw numbers.

---

## Scoring formulas (for reference)

**AE Team Score** = `[(Discoveries × 1) + (NBMs × 3)] ÷ Team Size + Bonus`  
**SDR Team Score** = `[(SALs × 1) + (SAOs × 3)] ÷ Team Size + Bonus`  
**Individual Score** = `(m1 × 1) + (m2 × 3) + Bonus`

---

## Shutting down after the competition

The site will keep running on Cloudflare's free tier indefinitely, but costs nothing either way.  
When you're done, go to **Workers & Pages** → your project → **Settings** → **Delete project**.
