# Pomodoro Timer

A single-page Pomodoro timer with daily session history, built with vanilla HTML, CSS, and JavaScript.

## Live Demo

> _Add your deployed URL here after deploying to GitHub Pages / Netlify / Vercel_

---

## How to Run Locally

No build tools or installs required. Just open the file:

```bash
# Option 1 — simplest
Open index.html directly in any browser (double-click it)

# Option 2 — local dev server (avoids any browser quirks)
npx serve .
# then open http://localhost:3000
```

### Fresh machine steps:
1. Download / clone this repository
2. Open `index.html` in Chrome, Firefox, or Safari
3. That's it — no npm install, no build step

---

## Features

- ⏱ Configurable focus (default 25 min) and break (default 5 min) durations
- ▶ Start, Pause, Resume, Reset, and Skip controls
- 🔔 Audible 3-tone chime when a session ends
- 🔄 Auto-transitions between focus and break phases
- 📋 Session history persisted via `localStorage`, resets each calendar day
- 📱 Fully responsive — works on 360px phones up to 1440px desktops
- ♿ Keyboard navigable, ARIA live regions, skip-to-content link
