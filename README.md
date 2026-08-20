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

## What 1–8 mean, while you are answering

An assessor used to get eight numbered buttons and nothing else. Once they had chosen,
they saw the level's **name**; the definition was in Setup, on another screen. So "is
this a 5 or a 6?" — the only question that matters at that moment — could not be
answered without leaving the questions. Over 350 questions nobody does that, so they
guess, and the scores are worth less.

The definitions are now at the point of answering, three ways:

- **On every button** as a tooltip and an accessible label — a mouse hover and a screen
  reader get the level and its meaning for free, with no visible weight on 350 cards
- **Under the scale**, the full definition of the level actually chosen, so what has just
  been asserted is legible. Before scoring, the two ends of the scale, so its direction
  is obvious
- **A "What 1–8 mean" panel** on the card: all eight with their definitions, the chosen
  one marked in the same colour as the button that chose it. Opening it is remembered
  across the whole area, so it is opened once rather than per question, and it opens for
  a lapsed licence too — it is help, not an edit

Two lines are reserved for the definition whether or not it needs both. Without that the
card's height followed whichever definition was showing, so correcting a 5 to a 6 moved
everything below it by a line, under the thumb of somebody halfway down a subject.

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

Comparing two assessments of the same kind over time is a different question from
holding self against external, and the labels follow: the column is **Change** rather
than Gap, the section is **Largest movements** rather than divergences, and the headline
figure is **Movement** rather than alignment. "Weakest areas" is ranked on the **later**
assessment — where they are weakest now, not where they were weakest before the work.

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

The Self and External apps cannot hold the two kinds against each other. Each is
single-purpose throughout: the word "external" does not appear in the Self app and "self"
does not appear in the External one. Each *can* compare two assessments **of its own
kind**, labelled by their titles, which is how a client holds last year's self assessment
against this year's without ever seeing the other kind. See
[DISTRIBUTING.md](DISTRIBUTING.md).

**All four editions produce the PDF report**, including a single assessment — headed with
that assessment's own title, one series, and no gap column or divergence section, because
there is no second assessment to diverge from. Until build 3.6 the two assessor editions
had no report button: it had been treated as the consolidator's output, which was wrong.

---

## Importing scores from a spreadsheet

**Setup → Import scores from a spreadsheet** takes a CSV. Two kinds work, because
there are two ways a spreadsheet of scores comes to exist:

- **The app's own CSV export, edited and brought back.** Export, change the score
  column in Excel, import it. This is the awkward one to support: the export names
  its score column after the *assessment title* — "Dec 2025 Review", not "Self
  assessment" — so the importer cannot look for a fixed heading and has to work out
  which column holds the scores.
- **A blank spreadsheet somebody filled in**, for a client whose scores already
  live elsewhere. **Download a blank spreadsheet** gives all 350 questions with
  empty `Score` and `Notes` columns, so nobody retypes question text and finds out
  later that it no longer matches.

Rows are matched on **question text**, which is unique across all 350, normalised
for whitespace and for the curly quotes Excel and Word insert unbidden. Row order
does not matter; an `Assessment Area` column, if present, is only a cross-check.
Notes come in alongside scores.

**Nothing is written until you say so.** The review step reports how many rows
matched, how many questions would be answered for the first time, how many existing
answers would *change*, and lists every row it skipped and why — a score of 9, a
score of "high", the same question twice, a question it does not recognise. Then
**Fill blanks only**, which never overwrites a typed answer, or **Import all**.
Either can be undone in one step.

A file it cannot understand is **refused whole**, not half-applied: no score column,
two candidate score columns, no question column, nothing matching. A partial import
is worse than none, because afterwards there is no way to tell which answers came
from where.

Two things it will not do. A locked edition refuses a spreadsheet whose score column
is headed with the other kind — an external export dropped into the Self app is
turned away by name rather than filed as self scores. And importing is a write, so a
lapsed licence cannot do it, though export still works.

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
check.html                which build each of the four apps is serving
check.webmanifest  check-*.png                 the checker's own install files
.nojekyll                 tells Pages to serve the files as-is
DISTRIBUTING.md           which app goes to whom, and what to tell assessors
INSTALLING.md             hosting, and installing on any platform
```

The self and external apps used to live here in `self/` and `external/`. They are now
repositories of their own.

Every edition is a **single self-contained HTML file** — questions, charts, logo and the
report generator all inside it. No build step, no dependencies, no server, no database.

---

## Installing it

It runs in **any modern browser** — Chrome, Edge, Safari, Firefox, on Windows, macOS,
Android, iPhone or iPad. Installing is optional; it gives the app its own icon, its own
window, and makes it work with no network.

| Where | How |
|---|---|
| **Windows / macOS**, Chrome or Edge | The **install icon** at the right-hand end of the address bar, or ⋮ → *Cast, save and share ▸ Install page as app* |
| **Android**, Chrome | ⋮ → **Install app** |
| **iPhone / iPad** | **Safari** → **Share** → **Add to Home Screen** |
| **Firefox** | No install button; it runs as an ordinary page, and still works offline |

The app puts an **Install this app** button in Setup when the browser offers one, so on
Chrome and Edge there is nothing to hunt for.

iPhone and iPad are the one place where the browser matters: **no iOS browser except
Safari can install a web app**. Chrome on iOS is Safari's engine with a different badge
and Apple does not expose the install to it. That restriction is Apple's, and applies
nowhere else.

---

## Who can see it

**A URL on its own shows nothing.** Without a licence key the app shows its logo, its name
and a key field — no questions, no dashboard, no tabs. The 350 questions are the product,
and "nobody knows the URL" is not a control.

**The link you send carries the key**, after the `#` so it never reaches the server or its
logs:

```
https://YOUR-USERNAME.github.io/REPO-NAME/#lic=CVM1.eyJj…
```

They open it and are straight in — nothing to paste. The key is stored on the device and
wiped from the address bar, so it is not left sitting in a screenshot or a shared tab.
A key pasted into the field by hand works exactly the same.

**Somebody whose key has lapsed is never locked out of their own work.** The gate is only
for a device with no licence *and* nothing on it. If there are answers here, they get the
read-only app instead, so they can always reach and export what they did.

This is why the repositories stay public and the URLs stay reachable: that is what lets
everyone get updates. The door is in the app, not in the hosting.

---

## Licensing

The client-facing apps — Self, External and the client app — carry a licence.
The consolidator does not; it is the operator's own tool.

**How it works.** You issue a signed key holding the client's name and an end date,
using `CVM Licence Keys (KEEP PRIVATE).html`, which never leaves your machine. They paste
it into **Setup → Licence**. The app verifies the signature against the public key built
into it, and works normally until the date passes.

**What happens at the end.** Scoring switches off. Everything already scored stays visible,
and export and the PDF report still work — nobody's completed assessment is held hostage
over a date. A new key pasted over the old one unlocks it again, with no rebuild and no
reinstall.

**What a key does and does not bind.** It binds a **date**, and that is enforced: the
signature cannot be forged, editing the expiry invalidates it, and the clock cannot be
wound back past what the device has recorded or past the build date. It does **not** bind
the client. The name in the key is a label, not a check — the client name in Setup is free
text, and typing a different one changes nothing about how long the licence runs. So a key
is a bearer token: anyone holding the link can use it, until the date.

That is a deliberate limit rather than an oversight, because the alternative is worse. A
name comparison would fail on "Acme" against "Acme Holdings PLC" and generate support
calls, and it would still be a check running on the visitor's own machine — friction, not
enforcement. **No licence can be enforced in a browser app**; what can be done is to make
the record accurate and misuse visible. So:

- The licence card names who the key was issued to, on screen.
- **The report footer records it too**, alongside the client name typed in Setup. If those
  two disagree, it is visible on the page — the document evidences the licence rather than
  just asserting a name somebody typed.
- **A key never travels inside a data file.** Exports and CSVs carry no key, and restoring
  a backup cannot install one. Before build 3.5 every JSON export contained a working
  signed key in plain text, so an assessor emailing their submission handed the key to
  whoever received it — and the consolidator collects those files from everybody. Keys
  travel by link from you, and only from you.

**With no key**, it depends on the app. The **Self** app is locked from the first screen and
tells the person to contact Manhattan CVLM for a key — nothing there should happen
unlicensed. The **External** and **client** apps run until the build's own date
(`BUILD_EXPIRES` in `build_editions.py`, currently 31 August 2027) and then go read-only.
Set `needs_key=True` on an edition in `build_editions.py` to move it to the stricter rule.

**What it does not do.** It cannot *enforce* anything, and no client-side app can. The code
is on their machine and the source is readable, so someone willing to edit the app can
remove the check. What the signing does is make a key impossible to forge or extend — the
private key exists only on your machine, and altering so much as the date inside a key
breaks its signature. The device clock cannot simply be wound back either: the latest date
the app has ever seen is stored, and time never runs backwards from it. Continuing past
expiry takes deliberate tampering, not forgetfulness.

If you ever need real enforcement, that means a server the app checks in with — which also
means it stops working offline, which is currently one of the better things about it.

---

## Turning on GitHub Pages

1. **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, folder: **`/ (root)`** → **Save**

The repository must be **public** for Pages on a free plan, and Pages has to be switched
on **per repository** — it is off by default on a new one.

---

## Checking what is deployed

Open **`check.html`** on the site:

```
https://YOUR-USERNAME.github.io/CVM-Assessment/check.html
```

It reads all four apps the way a visitor's browser does, ignoring every cache, and shows
which build each one is serving. If it shows you the assessment instead of a table, the file
has not been uploaded yet — before build 3.2 a missing path fell back to serving the app. A 404 against one of them almost always means Pages is
switched off for that repository. It answers "did that upload land?" in
one click, and it is the quickest way to tell a hosting problem from a phone that is
holding onto a cached copy.

**It installs to a home screen too**, so it is one tap rather than a bookmark. It carries
the same Manhattan CVLM mark as the four apps with a **green band** along the bottom of the
icon, so it sits alongside them without being mistaken for one. It has a manifest of its
own (`check.webmanifest`) precisely so the installed icon opens the checker — an icon built
from the app's manifest would open the assessment instead, which is exactly the confusion
worth avoiding.

`check.html`, its manifest and its icons are all produced by the build alongside the four
apps; they are not maintained by hand in this repository. Upload all six files together.

---

## How an update reaches people

**The page is fetched from the network first**, with the cached copy as the fallback. So a
reload with any signal at all lands on the current version — one reload, not two — and the
app still opens instantly with no network.

That is a change from how it worked up to build 2.5, which served the cached copy every
time and fetched the new one quietly for next time. The first visit after an update showed
the old app, which reads exactly like the update having failed.

**A release made while somebody has the app open** shows them *"A new version is ready"*
with a Reload now button. Nothing is swapped underneath them mid-question, and their
answers survive either way.

**Installed copies check on every open**, and the worker script itself is exempt from the
browser's HTTP cache (`updateViaCache: "none"`), so a new release cannot sit unnoticed
behind a stale copy of `sw.js`.

**If the site is ever switched off** — Pages turned off, the repository made private on a
free plan, a deploy that fails — an installed copy keeps working from its cache. The worker
treats a 404 or a 500 as a failed fetch, not as the app, so a hosting error never replaces
a working install. New visitors get nothing, of course.

To release: replace `index.html`, **bump `const CACHE` in `sw.js`**, commit. Without the
bump, devices keep the assets they cached — the page will be current but its icons and
manifest will not.

If a device is somehow still stuck: in the browser, clear site data for the URL; on a
home-screen install, delete the icon and add it again. Answers live in the browser's
storage for that origin and survive the first; the second clears them, so export first.

---

## Releasing a change

1. Replace `index.html`
2. **Bump `const CACHE` in `sw.js`** — currently `cvm-assess-v29`. The other repositories
   carry their own: `cvm-self-v24`, `cvm-external-v24`, `cvm-consolidate-v22`
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

Deleting the installed app, or clearing site data in the browser, clears that app's
answers. Export at the end of every session.

An export is named for the client, the assessment title, the date **and the assessor**, so
several contributors' files stay apart in one download folder instead of arriving as
"(1)", "(2)" copies of one name.

---

## Notes

The 1–8 scale level names ("Not in place" … "Optimised") are a generic maturity ladder;
the source workbook carried no definitions. They appear in the method section of every
client report — replace them in the `LEVELS` array near the top of the script in each
`index.html`.

---

© Brooklyn Solutions AI / Manhattan CVLM. All rights reserved. Published here for
distribution to named clients; not licensed for reuse or redistribution.
