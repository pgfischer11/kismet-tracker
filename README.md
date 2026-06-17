# Kismet House Tracker

A family achievement tracker PWA for the Kismet beach house on Fire Island, NY.

## Stack
- Vanilla HTML/CSS/JS (single file app)
- Firebase Realtime Database (live sync)
- Netlify (hosting, auto-deploys from `main`)

## Structure
```
src/          ← deployed to Netlify
  index.html  ← entire app
  manifest.json
  sw.js
  icon-192.png
  icon-512.png
docs/         ← product docs
```

## Deploy
Push to `main` → Netlify auto-deploys from `src/` within ~60 seconds.

```bash
git add .
git commit -m "Description"
git push
```

## Local Storage Keys
- `kismet_v5` — app state (do not change)
- `kismet_who_v1` — user identity (do not change)
