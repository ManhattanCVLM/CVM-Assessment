# Which app goes to whom

| App | Repository / URL | Who gets it |
|---|---|---|
| **Self** | `CVM-Assessment-Self` | The client's own people |
| **External** | `CVM-Assessment-Ext` | You and any external reviewers |
| **Client** | `CVM-Assessment` | The client — self against external, or two assessments compared |
| **Consolidator** | `CVM-Consolidation` | You only — combining submissions, and the client report |

Four repositories, four Pages sites, four home-screen apps. An assessor gets a URL that
does one job and cannot be pointed at another.

## Why the assessor editions are locked

The lane is fixed at build time. There is no Self/External switch to leave in the wrong
position, so a submission physically cannot arrive filed against the wrong assessment —
which is the one mistake the merge could not detect on its own.

From build 2.0 they are single-purpose all the way through, not just locked at the switch.
The Self edition never says "external" anywhere and the External edition never says
"self":

- no **Gap ≥ 2** filter and no **Previous gap** jump — both compare the two assessments
- one score chip per subject instead of a pair with a permanent dash
- the other assessment's score, note and comment never appear on a question card
- the CSV has one score column and no gap column
- the dashboard plots one series, with no alignment card and no divergence list

If a file holding both assessments is restored into one of them — the only way the other
side's scores can reach that device — the data stays in the file but never surfaces, and
**the export drops it**. Otherwise the consolidator would merge answers into an assessment
that person was never asked to do, in their name.

They also **start completely empty**, as does the client app. Each
assessment is a different client, so nothing is preloaded; the full edition can load the
workbook's sample data deliberately from **Setup → Reset**.

Neither assessor app can combine submissions. Each can produce a PDF **of a comparison**
it has loaded, but not of a scoring session — a submission goes back to you as JSON, and
the client report comes from the consolidator.

## Telling them apart

All four carry the Manhattan CVLM logo. The **app icons** differ by a colour band along
the bottom: blue for Self, orange for External, none for the client app, navy for the
consolidator. On a home screen they read as "CVM Self" and "CVM External".

Each keeps its own separate storage and its own offline cache, so both can be installed
on one device without interfering.

## Comparing two of the same kind

Each assessor app can also **compare two assessments of its own kind** — Setup → *What
this copy is doing* → *Comparing two self assessments*. Import last year's and this
year's; every chart and table is labelled by their titles, and the PDF report becomes
available. A file from the other kind is refused rather than loaded under the wrong name.

So the client can hold self against self, or external against external, without either
app ever mentioning the other kind. Holding **self against external** is the client app's
job, in `CVM-Assessment`.

## Hosting them

```
/CVM-Assessment/         → the client app (yours and theirs)
/CVM-Assessment-Self/    → hand this URL to the client's people
/CVM-Assessment-Ext/     → hand this URL to external reviewers
/CVM-Consolidation/      → yours alone
```

Pages is switched on **per repository** — a new repo has it off, and the site 404s until
you turn it on under Settings → Pages.

Each person opens their URL in whatever browser they use. On Chrome or Edge the app
offers an **Install this app** button in Setup; on an iPhone or iPad it is
**Safari → Share → Add to Home Screen**, because Apple allows no other iOS browser to
install a web app. Installing is optional — it works as an ordinary page either way.

The `.html` files alongside are single-file copies of the same two editions, for emailing
or opening on a laptop. They work, but with no home-screen icon and less reliable
storage — the hosted route is the one for real assessments.

## What to tell each assessor

1. Open the link, and install it if offered (its own icon, and it works offline)
2. In **Setup**, enter **your name**, and the **client** and **assessment title** exactly
   as given to you — the app nags until all three are filled
3. Answer the sections you have been asked to cover; leave the rest blank
4. **Setup → Export my assessment (JSON)** and send the file back

Give everyone the identical client and title. If they differ, the submission still
imports but is flagged as belonging to a different assessment, and you confirm it by hand.
