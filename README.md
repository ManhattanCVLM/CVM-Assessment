# CVM Maturity Assessment

Contract Value Management maturity assessment as offline mobile apps. 350 questions across
7 assessment areas, scored 1–8, with self versus external comparison, charts, multi-assessor
consolidation and a client-ready PDF report.

Built from `CVM Maturity Assessment Comparison v1.0.xlsm`.

---

## Three editions, one Pages site

| Edition | URL | Who it's for |
|---|---|---|
| **Comparison** (full) | `/` | You — both lanes, combining submissions, the client report |
| **Self** | `/self/` | The client's own people — locked to the self assessment |
| **External** | `/external/` | External reviewers — locked to the external assessment |

Once Pages is on, those are:

```
https://YOUR-USERNAME.github.io/cvm-assessment/
https://YOUR-USERNAME.github.io/cvm-assessment/self/
https://YOUR-USERNAME.github.io/cvm-assessment/external/
```

Each installs to the home screen as its **own separate app**, with its own icon, its own
offline cache and its own stored answers. They share an origin but not a storage key, so
scores entered in one never appear in another — verified by running all three side by side.

The assessor editions have no lane switch, so a submission cannot arrive filed against the
wrong assessment. They also cannot combine submissions or generate a client report; those
belong to the comparison edition. See [DISTRIBUTING.md](DISTRIBUTING.md).

---

## Layout

```
index.html                the comparison edition
sw.js  manifest.webmanifest  icon-*.png
self/                     the self edition, complete and self-contained
external/                 the external edition
.nojekyll                 tells Pages to serve the files as-is
DISTRIBUTING.md           which edition goes to whom, and what to tell assessors
MULTI-ASSESSOR-WORKFLOW.md  collecting and merging submissions, and the PDF report
INSTALL-ON-IPHONE.md      hosting and home-screen install
ios/                      optional native iOS wrapper (Xcode project)
```

Every edition is a **single self-contained HTML file** — the questions, the charts, the
logo and the report generator are all inside it. No build step, no dependencies, no
server, no database.

---

## Turning on GitHub Pages

1. **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, folder: **`/ (root)`** → **Save**
4. Wait a minute, then open the three URLs above

The repository must be **public** for Pages on a free plan. One Pages site serves all
three editions — there is nothing to configure per folder.

---

## Releasing a change

1. Replace the `index.html` you are changing (root, `self/`, or `external/`)
2. **Bump the cache version in that edition's `sw.js`** — each has its own:
   `cvm-assess-v5` at the root, `cvm-self-v2` in `self/`, `cvm-external-v2` in `external/`
3. Commit and push; Pages redeploys within a minute

Step 2 is not optional. Without it, devices that already installed that edition keep
serving their cached copy. Answers already on a device survive an update.

---

## Where the data lives

There is no server side. Every score and note is stored on the device that entered it and
is never transmitted. Two consequences:

- **Publishing this repository publishes the blank tools, not anyone's results.** The
  questions are in the HTML; the answers are not.
- **Each device holds its own assessment.** To combine them, each assessor exports JSON
  from Setup and sends it to you; the comparison edition merges them.

Nothing is preloaded in any edition — each assessment is a different client, and shipping
one client's scores inside the app would risk them appearing in another's report. The
comparison edition can load the workbook's sample data on demand from **Setup → Reset**.

Deleting a home-screen icon, or "Clear History and Website Data" in Safari, clears that
edition's answers. Export at the end of every session.

---

## Notes

The 1–8 scale level names ("Not in place" … "Optimised") are a generic maturity ladder;
the source workbook carried no definitions. They appear in the method section of every
client report — replace them in the `LEVELS` array near the top of the script in each
`index.html`.

The Manhattan CVLM logo was reconstructed from a photograph, so it is soft at large sizes.
Replacing it with original artwork means swapping the `LOGO` data URI in each `index.html`.
