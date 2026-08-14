# The three editions

| Edition | Who gets it | Lane |
|---|---|---|
| **Self** (`self/`) | The client's own people | Locked to **self assessment** |
| **External** (`external/`) | You and any external reviewers | Locked to **external assessment** |
| **Full** (`index.html` in the parent folder) | You only | Both lanes, plus combining and the client report |

## Why the assessor editions are locked

The lane is fixed at build time. There is no Self/External switch to leave in the wrong
position, so a submission physically cannot arrive filed against the wrong assessment —
which is the one mistake the merge could not detect on its own.

They also **start completely empty**, as does the full edition from build 1.4. Each
assessment is a different client, so nothing is preloaded; the full edition can load the
workbook's sample data deliberately from **Setup → Reset**.

Neither assessor edition can combine submissions or produce a client report — those are
yours. They export their own JSON and CSV, which is all they need to do.

## Telling them apart

All three carry the Manhattan CVLM logo. The **app icons** differ by a colour band along
the bottom: blue for Self, orange for External, none for the full edition. On a home
screen they read as "CVM Self" and "CVM External".

Each keeps its own separate storage and its own offline cache, so both can be installed
on one device without interfering.

## Hosting them

```
/cvm/            → the full edition (yours)
/cvm/self/       → hand this URL to the client's people
/cvm/external/   → hand this URL to external reviewers
```

Each person opens their URL in **Safari** → **Share** → **Add to Home Screen**.

The `.html` files alongside are single-file copies of the same two editions, for emailing
or opening on a laptop. They work, but with no home-screen icon and less reliable
storage — the hosted route is the one for real assessments.

## What to tell each assessor

1. Open the link in Safari and add it to your home screen
2. In **Setup**, enter **your name**, and the **client** and **assessment title** exactly
   as given to you — the app nags until all three are filled
3. Answer the sections you have been asked to cover; leave the rest blank
4. **Setup → Export my assessment (JSON)** and send the file back

Give everyone the identical client and title. If they differ, the submission still
imports but is flagged as belonging to a different assessment, and you confirm it by hand.
