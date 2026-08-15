# CVM Maturity Assessment

Contract Value Management maturity assessment as offline mobile apps. 350 questions across
7 assessment areas, scored 1–8, with charts and a client-ready PDF report.

Built from `CVM Maturity Assessment Comparison v1.0.xlsm`.

---

## Four apps, four repositories

| Repository | URL | Who it's for |
|---|---|---|
| **CVM-Assessment** (this one) | `/CVM-Assessment/` | The client — self against external, or two assessments compared |
| **CVM-Assessment-Self** | `/CVM-Assessment-Self/` | The client's own people — the self assessment alone |
| **CVM-Assessment-Ext** | `/CVM-Assessment-Ext/` | External reviewers — the external assessment alone |
| **CVM-Consolidation** | `/CVM-Consolidation/` | You — combining many assessors into one assessment |

Each is a separate repository with its own Pages site, so an assessor is handed a URL that
does one job. They share the `github.io` origin but keep **separate storage keys and
separate offline caches**, so all four can be installed on one device and none can see
another's answers.

That shared origin has one useful consequence: an assessor who was using the old
`/CVM-Assessment/self/` URL and moves to `/CVM-Assessment-Self/` **still sees their
scores**, because the storage key did not change with the path.

---

## Scoring 350 questions

Scoring a question scrolls the next one into view, so a full pass through an area is
taps rather than taps and scrolls. It moves only on a question that had **no** score
yet — changing one you have already given never pulls the page out from under you — and
it ignores a score tap that arrives while the page is still travelling, so a fast second
tap cannot land on the question that just slid into that spot. The end of a subject opens
the next one. **Setup → While you score** turns it off.

---

## What the dashboard is called

The dashboard and the report name themselves after the data they hold, not after
the slots it sits in:

| What is loaded | What you see |
|---|---|
| One assessment | Its own **assessment title** on the heading, the chart legend and the table column — one series, no empty second column and no gap figures |
| Self and external together | **Self** and **External**, two series, alignment and the largest gaps |
| Two assessments compared | **Both assessment titles**, one against the other |

So an assessor running a single self assessment sees "2026 Baseline Review" on
their charts rather than the word "Self" beside an empty "External".

---

## Assessment type

The client app's **Setup → Assessment type** offers three:

**Self assessment** — score the self side.

**External assessment** — score the external side.

**Comparison of two assessments** — scoring switches off; import two finished
assessments and every chart, table and the report compares them, **labelled by their own
assessment titles** rather than "Self" and "External". This is how a client holds last
year's assessment against this year's.

The Self and External apps — now in their own repositories — cannot do this. Each is
single-purpose throughout: the word "external" does not appear in the Self app and "self"
does not appear in the External one. Each *can* compare two assessments **of its own
kind**, labelled by their titles, which is how a client holds last year's self assessment
against this year's without ever seeing the other kind. See
[DISTRIBUTING.md](DISTRIBUTING.md).

---

## What is deliberately not here

**Combining many contributors' submissions.** That lives in the separate
`CVM-Consolidation` repository, and its merging engine is **stripped from these builds at
build time** — absent, not hidden behind a flag. The build asserts it on the built file of
every edition, so a wiring mistake fails the build rather than shipping. These two
repositories have separate histories for the same reason: one shared history would leave
merge-capable code recoverable from an earlier commit.

---

## Layout

```
index.html                the client app
sw.js  manifest.webmanifest  icon-*.png
.nojekyll                 tells Pages to serve the files as-is
DISTRIBUTING.md           which app goes to whom, and what to tell assessors
INSTALL-ON-IPHONE.md      hosting and home-screen install
```

The self and external apps used to live here in `self/` and `external/`. They are now
repositories of their own.

Every edition is a **single self-contained HTML file** — questions, charts, logo and the
report generator all inside it. No build step, no dependencies, no server, no database.

---

## Turning on GitHub Pages

1. **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, folder: **`/ (root)`** → **Save**

The repository must be **public** for Pages on a free plan, and Pages has to be switched
on **per repository** — it is off by default on a new one.

---

## Releasing a change

1. Replace `index.html`
2. **Bump `const CACHE` in `sw.js`** — currently `cvm-assess-v12`. The other repositories
   carry their own: `cvm-self-v7`, `cvm-external-v7`, `cvm-consolidate-v5`
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

An export is named for the client, the assessment title, the date **and the assessor**, so
several contributors' files stay apart in one download folder instead of arriving as
"(1)", "(2)" copies of one name.

---

## Notes

The 1–8 scale level names ("Not in place" … "Optimised") are a generic maturity ladder;
the source workbook carried no definitions. They appear in the method section of every
client report — replace them in the `LEVELS` array near the top of the script in each
`index.html`.
