# CVM Maturity Assessment

A Contract Value Management maturity assessment as an offline mobile app. 350 questions
across 7 assessment areas, scored 1–8 by both a self assessor and an external assessor,
with a comparison dashboard and CSV export back into Excel.

Built from `CVM Maturity Assessment Comparison v1.0.xlsm`.

**Live app:** https://YOUR-USERNAME.github.io/cvm-assessment/
*(replace with your own once GitHub Pages is switched on — see below)*

---

## What's here

```
index.html                the entire app: questions, scoring, charts, export
sw.js                     service worker — caches the app for offline use
manifest.webmanifest      home-screen name, icon and full-screen behaviour
icon-*.png                app icons
.nojekyll                 tells GitHub Pages to serve the files as-is
INSTALL-ON-IPHONE.md      how to get it onto a phone or iPad
ios/                      optional native iOS wrapper (Xcode project)
```

The web app is a **single self-contained HTML file**. No build step, no dependencies, no
server, no database. Edit `index.html` and commit; that's a release.

---

## Turning on GitHub Pages

1. **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, folder: **`/ (root)`** → **Save**
4. Wait a minute, then open `https://YOUR-USERNAME.github.io/cvm-assessment/`

The repository must be **public** for Pages on a free plan.

Then on the iPhone or iPad: open that URL in **Safari** → **Share** → **Add to Home
Screen**. Full instructions in [INSTALL-ON-IPHONE.md](INSTALL-ON-IPHONE.md).

---

## Releasing a change

1. Edit `index.html`
2. **Bump the cache version in `sw.js`** — `const CACHE = "cvm-assess-v2"` → `"v3"`
3. Commit and push

Step 2 is not optional. Without it, devices that already installed the app keep serving
their cached copy and never see the update. Answers already saved on a device are not
affected by an update.

---

## Where the data lives

There is no server side. Every score and note is stored in `localStorage` on the device
that entered it and is never transmitted anywhere. That has two consequences:

- **Publishing this repository publishes the blank tool, not anyone's results.** The
  questions are in `index.html`; the answers are not.
- **Each device holds its own separate assessment.** To combine or archive them, use
  **Setup → Export JSON backup** (or CSV) on each device.

Deleting the home-screen icon, or "Clear History and Website Data" in Safari, clears the
saved answers. Export at the end of every session.

---

## The native iOS app (optional)

`ios/` holds an Xcode project that wraps the same `index.html` in a real native app —
its own binary and icon, exports through the iOS share sheet, haptics on scoring. Needed
only if you want App Store or TestFlight distribution. See [ios/START HERE.md](ios/START%20HERE.md).

If you change the web app, copy the new `index.html` over `ios/CVMAssessment/Web/index.html`
to keep the native build in step.

---

## Notes on the source data

The workbook's Manhattan Services figures are preloaded so the dashboard is populated on
first open. **Setup → Start a blank assessment** clears it; **Reload workbook data** puts
it back.

In that source data, six of the seven areas have identical self and external scores — the
workbook's import macros appear to have read the same source file into both columns. Only
Core Supply Chain shows genuine divergence, which is why most areas read "aligned". Re-run
the workbook's two imports against the correct source files to fix it.

The 1–8 scale level names ("Not in place" … "Optimised") are a generic maturity ladder;
the workbook carried no definitions. Replace them in the `LEVELS` array near the top of
the script in `index.html` with your own.
