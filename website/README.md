# Eloquent Dark Pro — Promo Site

Static landing page for the **Eloquent Dark Pro** VS Code theme. Pure HTML + CSS, zero build step.

## Local preview

Just open `index.html` in your browser, or:

```bash
npx serve .
```

## Deploy to Vercel

### Option A — One-click via the dashboard
1. Push this repo to GitHub
2. Go to <https://vercel.com/new> and import the repo
3. Set **Root Directory** to `website`
4. Framework preset: **Other** · Build command: *(leave blank)* · Output directory: *(leave blank)*
5. Click **Deploy**

### Option B — From the CLI
```bash
npm i -g vercel
cd website
vercel        # follow prompts, accept defaults
vercel --prod # promote to production
```

## Files
- `index.html` — landing page
- `styles.css` — palette + layout
- `vercel.json` — caching headers
- `screenshots/` — copied from the repo root

## Updating screenshots
Replace files in `website/screenshots/` (originals live in `../screenshots/`).
