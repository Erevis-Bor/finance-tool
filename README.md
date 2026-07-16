# Runway — a goals-first money planner (demo v1)

A single-file web app for couples planning shared savings goals. It doesn't just
track balances — it tells you whether the things you're saving for will actually
happen on time, and what to change if they won't.

**[Live demo →](#)** &nbsp;·&nbsp; *(enable GitHub Pages on this repo, then drop the link here)*

This is a **demo build**. It opens on a fictional couple (Sam & Riya) with sample
data so you can see a populated plan immediately:

- **↻ Reset demo** reloads the sample data at any time.
- **✨ Start setup** drops into the guided setup flow to build a plan from your own
  numbers.

Everything runs in the browser and is saved only in `localStorage` under a
demo-specific key — nothing leaves the device, and the demo can't touch a real
saved plan.

## What's interesting about it

The core is a **claim-based waterfall engine**. Every goal becomes one or more
dated *claims* on a shared pool of money, and claims are funded in **deadline
order** — the nearest deadline draws from the pool first, and later goals compete
for whatever's left. That one rule drives everything the app can say:

- **Staged goals.** A goal like the sample *Ski trip* splits into dated payments
  (flights, lodge, spending money), each its own claim with its own shortfall, so
  "flights funded, lodge £200 short" is a state the app can actually represent.
- **Live status.** Each goal reads out in plain language — *on track* or
  *£X short by [month]* — recomputed from the same engine.
- **One-tap fixes.** When a goal is at risk, the app offers concrete fixes:
  raise the joint saving rate to the exact minimum that clears everything (found
  by binary search), or push a single goal's deadline to the nearest month it
  lands on time.
- **Personal goals and a savings challenge.** Alongside the joint pool, each
  person has their own goals, including a 365-day penny-style challenge simulated
  day by day with interest.

## How it's built

- **One file, no build step.** React and the compiled UI are inlined into
  `index.html`; there is no bundler and no CDN dependency to fail.
- **A pure engine under the components.** The waterfall, status, min-monthly
  solver, and challenge simulation are plain functions, kept separate from
  rendering and unit-testable on their own.
- **Built conversationally with Claude** — designing the engine, arguing the
  architecture through several rebuilds, and testing against edge cases (staged
  goals, challenges, past-due deadlines) rather than writing boilerplate by hand.

## Run it

Open `index.html` in any browser. To publish: push to GitHub, enable Pages
(Settings → Pages → deploy from `main`, root), and paste the resulting URL above.
