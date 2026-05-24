# ANSWERS.md

## 1. How to Run

No installation needed. Open `index.html` in any browser directly.

For a local server:
```bash
npx serve .
# Visit http://localhost:3000
```

Deployed URL: _(add after deploying)_

---

## 2. Stack & Design Choices

**Stack:** Vanilla HTML, CSS, and JavaScript — no frameworks, no build step.

I chose vanilla JS because the app has a single responsibility (a timer), and adding React or Vue would introduce unnecessary complexity. A single `index.html` also makes it trivially easy for anyone to open and verify — which matters for a submission that will be reviewed by real people.

**Two specific design decisions:**

**① The SVG ring takes up ~60% of the viewport height.**
I made the countdown ring the largest element on screen — not for decoration, but because the primary question a user has while working is "how much time is left?" The ring answers that at a glance without needing to read numbers precisely. The ring progress arc depletes smoothly over the full session duration, so even peripheral vision can register "nearly done" vs "still plenty of time."

**② Color and glow shift entirely when switching between focus and break.**
Rather than just changing a label, the whole interface changes color — red-pink for focus, teal for break, amber for paused. This means a user who glances at the app tab (which also updates its title) gets instant context without reading anything. The `body` class switch (`break-mode`, `paused`) drives all color changes through CSS variables, keeping the logic in one place.

---

## 3. Responsive & Accessibility

**Responsive behaviour:**
- At 360px (phone): the ring shrinks to 230px via a CSS custom property override in a `max-width: 400px` media query. Buttons stack naturally in flexbox. Font sizes use `clamp()` so they scale fluidly between breakpoints without jumping.
- At 1440px (laptop): the ring grows to 320px and the app gains more top padding, using the extra vertical space for breathing room rather than forcing content to spread horizontally (there's no reason to go two-column on a timer).

**Accessibility handled:**
- A "Skip to timer" link appears on keyboard focus, letting keyboard users jump past the header.
- The mode pill has `role="status"` and `aria-live="polite"` so screen readers announce phase changes (Focus → Break) without interrupting the user.
- The history list has `role="log"` and `aria-live="polite"` so new session entries are announced when they appear.
- All interactive controls are `<button>` elements (not divs), so they're natively keyboard-focusable and have visible `:focus-visible` outlines.
- Color contrast: all text meets WCAG AA against the dark backgrounds.

**Accessibility skipped:**
- I did not implement a full ARIA timer pattern (`aria-valuenow`, `aria-valuemin`, `aria-valuemax` on the countdown). Screen readers would currently only read the timer when it changes because of the `role="timer"` attribute, but a proper implementation would also expose the numeric value as a range widget. I skipped this because it requires more nuanced testing with actual screen reader software (NVDA, VoiceOver) and I didn't want to add half-correct ARIA that could confuse assistive tech more than no ARIA at all.

---

## 4. AI Usage

I used Claude (claude.ai) during this project in the following places:

**① SVG ring progress math**
I asked: *"How do I animate a circular progress ring in SVG using stroke-dashoffset?"*
It gave me the formula `offset = circumference × (1 − ratio)` and a working SVG snippet.
**What I changed:** The AI's example used a fixed `r="45"` and the circumference was hardcoded. I switched to `r="44"` to add visual padding inside the SVG viewBox, and computed `CIRCUMFERENCE = 2 * Math.PI * 44` as a constant at the top of the script so it's easy to adjust if the radius changes.

**② Web Audio beep**
I asked: *"How do I play a beep sound in JavaScript without an audio file?"*
It gave me a single oscillator tone.
**What I changed:** The AI's output played one flat beep at 440Hz. I changed it to play three ascending tones (520Hz, 660Hz, 800Hz) staggered by 0.18s each, which feels like a satisfying "ding-ding-ding" completion sound instead of a harsh alarm. I also wrapped it in a try/catch because browsers block `AudioContext` creation before a user gesture — without the catch, a console error would appear on first load.

**③ localStorage date-reset logic**
I asked: *"How do I check if a stored date is from a previous day and reset the data?"*
It gave me a date comparison using `new Date().toDateString()`.
**What I changed:** I switched to `new Date().toISOString().slice(0, 10)` (which returns `"YYYY-MM-DD"`) because ISO format is timezone-stable and sorts lexicographically — `toDateString()` returns locale-dependent strings like `"Mon Jun 03 2024"` which could theoretically differ across environments.

---

## 5. Honest Gap

**What's not polished enough:** The settings inputs (focus/break duration) are not locked while the timer is running — I disable the *effect* (duration changes don't apply mid-session), but I don't visually disable or grey out the inputs. A user could type in the fields while the timer runs and see nothing happen, which is confusing.

**What I'd do with another day:** I'd add a `disabled` attribute to both inputs when the timer starts, and re-enable them on reset. I'd also add a small tooltip or inline message ("Changes apply after reset") that appears if the user clicks a disabled input, so they understand why nothing changed rather than assuming the app is broken.
