# CVM Maturity Assessment

Contract Value Management maturity assessment as offline mobile apps. 350 questions across
7 assessment areas, scored 1–8, with charts and a client-ready PDF report.

Built from `CVM Maturity Assessment Comparison v1.0.xlsm`.

---

## Three editions, one Pages site

| Edition | URL | Who it's for |
|---|---|---|
| **Client app** | `/` | The client — score, or compare two assessments, and produce the report |
| **Self** | `/self/` | The client's own people — locked to the self assessment |
| **External** | `/external/` | External reviewers — locked to the external assessment |

Once Pages is on:

```
https://YOUR-USERNAME.github.io/CVM-Assessment/
https://YOUR-USERNAME.github.io/CVM-Assessment/self/
https://YOUR-USERNAME.github.io/CVM-Assessment/external/
```

Each installs to the home screen as its **own separate app**, with its own icon, offline
cache and stored answers. They share an origin but not a storage key, so scores in one
never appear in another.

---

## Assessment type

The client app's **Setup → Assessment type** offers three:

**Self assessment** — score the self side.

**External assessment** — score the external side.

**Comparison of two assessments** — scoring switches off; import two finished
assessments and every chart, table and the report compares them, **labelled by their own
assessment titles** rather than "Self" and "External". This is how a client holds last
year's assessment against this year's.

The Self and External editions have this locked and no switch at all, so a submission
cannot arrive filed against the wrong side.

---

## What is deliberately not here

**Combining many contributors' submissions.** That lives in the separate, private
`CVM-Consolidation` repository, and its merging engine is **stripped from these builds at
build time** — absent, not hidden behind a flag. These two repositories have separate
histories for the same reason: one shared history would leave merge-capable code
recoverable from an earlier commit.

---

## Layout

```
index.html                the client app
sw.js  manifest.webmanifest  icon-*.png
self/                     the self edition, complete and self-contained
external/                 the external edition
.nojekyll                 tells Pages to serve the files as-is
DISTRIBUTING.md           which edition goes to whom, and what to tell assessors
INSTALL-ON-IPHONE.md      hosting and home-screen install
```

Every edition is a **single self-contained HTML file** — questions, charts, logo and the
report generator all inside it. No build step, no dependencies, no server, no database.

---

## Turning on GitHub Pages

1. **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, folder: **`/ (root)`** → **Save**

The repository must be **public** for Pages on a free plan. One Pages site serves all
three editions; there is nothing to configure per folder.

---

## Releasing a change

1. Replace the `index.html` you are changing (root, `self/`, or `external/`)
2. **Bump the cache version in that edition's `sw.js`** — each has its own:
   `cvm-assess-v7` at the root, `cvm-self-v2` in `self/`, `cvm-external-v2` in `external/`
3. Commit and push; Pages redeploys within a minute

Without step 2, devices that already installed that edition keep serving their cached copy.
Answers already on a device survive an update.

---

## Where the data lives

There is no server side. Every score and note is stored on the device that entered it and
is never transmitted. So **publishing this repository publishes the blank tools, not
anyone's results** — the questions are in the HTML; the answers are not.

Nothing is preloaded in any edition: each assessment is a different client, and shipping
one client's scores inside the app would risk them appearing in another's report.

Deleting a home-screen icon, or "Clear History and Website Data" in Safari, clears that
edition's answers. Export at the end of every session.

---

## Notes

The 1–8 scale level names ("Not in place" … "Optimised") are a generic maturity ladder;
the source workbook carried no definitions. They appear in the method section of every
client report — replace them in the `LEVELS` array near the top of the script in each
`index.html`.
