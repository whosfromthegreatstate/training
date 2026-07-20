# Training

A single-page workout viewer. Opens to today, one tap per exercise for cues and a demo.

Static files only — no build step, no dependencies, no server.

---

## Deploy with Claude Code

From inside this folder:

```
claude
```

Then paste:

> Create a new private GitHub repo called `training`, commit everything in this
> directory, push to `main`, and enable GitHub Pages serving from the root of the
> `main` branch. Tell me the live URL when it's up.

Pages usually goes live within a minute or two at
`https://<your-username>.github.io/training/`

**Note:** GitHub Pages serves *public* content even from a private repo only on paid
plans. On the free tier, Pages requires a public repo. Nothing here is sensitive —
it's a workout plan — but if you'd rather keep it private, Cloudflare Pages and
Netlify both host private repos free and take the same static files.

## Add to your home screen

**iOS** — open the URL in Safari (not Chrome), Share → Add to Home Screen.
**Android** — open in Chrome, menu → Add to Home screen / Install app.

It runs full-screen with no browser chrome, and works offline after the first load.

---

## Re-syncing the rotation

The program is a **7-day rotation**, not a weekly schedule. It advances one position
per calendar day and floats against the week.

| Position | Session |
|---|---|
| 1 | Squat + Jump |
| 2 | Bench + Pull |
| 3 | Landings + Frontal Plane |
| 4 | Rest / easy aerobic |
| 5 | Deadlift + Posterior |
| 6 | Skill Day |
| 7 | Rest |

Anchored to **Sun 19 Jul 2026 = position 5 (Deadlift)**.

Miss a day, travel, or want to shift the cycle? Edit two lines near the top of the
script block in `index.html`:

```js
const ANCHOR_DATE = new Date(2026,6,19);   // months are 0-indexed: 6 = July
const ANCHOR_POS  = 5;
```

Set `ANCHOR_DATE` to today and `ANCHOR_POS` to whatever session you're doing today.
Everything downstream recalculates.

---

## Phases

Read from the date automatically — nothing to toggle.

| Window | Shows |
|---|---|
| through 8 Aug 2026 | Phase 0 · Baseline |
| 9–21 Aug 2026 | Iceland · Deload |
| from 24 Aug 2026 | Block 1 · Week N |

Two drills stay locked until the tendons have had time: **single-leg landings**
unlock at week 4, **off-axis landings** at week 8. They appear on their own.

Week numbers count from `START` (24 Aug 2026), also editable at the top of the script.

---

## Editing the program

Everything lives in the `DAYS` object in `index.html`. One entry per exercise:

```js
{
  n: "Deadlift",              // name
  load: "5×3",                // the big number
  unit: "265–285",            // small label under it
  new: true,                  // shows a NEW tag — movements unfamiliar to you
  week: 4,                    // optional: locked until this week of Block 1
  sub: "One line under the name.",
  cues: ["Tap-to-expand bullets.", "<strong>Bold</strong> works."],
  demo: "https://..."         // optional link
}
```

Bump `CACHE` in `sw.js` after editing, or the service worker will keep serving the
old version.

---

## What this is not

Not a tracker. Sets, reps, and weights go in **Progression**. Milestones, phase
decisions, and the reasoning behind the programming live in **Asana**.

This file answers one question: what do I do right now.
