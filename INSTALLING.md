# CVM Maturity Assessment — hosting and installing it

This folder is the whole app. Put these files on any web address and open it in **any
modern browser** — Chrome, Edge, Safari or Firefox, on Windows, macOS, Android, iPhone or
iPad. It runs the same everywhere.

**Installing is optional.** It gives the app its own icon and its own window, and makes it
work with no network at all:

| Where | How |
|---|---|
| **Windows / macOS**, Chrome or Edge | The **install icon** at the right-hand end of the address bar, or ⋮ → *Cast, save and share ▸ Install page as app* |
| **Android**, Chrome | ⋮ → **Install app** |
| **iPhone / iPad** | **Safari** → **Share** → **Add to Home Screen** |
| **Firefox** | No install button; it runs as an ordinary page and still works offline |

The app shows an **Install this app** button in Setup whenever the browser offers one, so
on Chrome and Edge there is nothing to go looking for.

The iPhone section below is the long version, because iOS is the one platform where the
browser matters — Apple allows no iOS browser except Safari to install a web app.

```
index.html                 the app (all 350 questions, dashboard, export)
manifest.webmanifest       tells iOS the name, icon and full-screen behaviour
sw.js                      caches the app on the device so it runs offline
icon-192.png  icon-512.png  icon-512-maskable.png  apple-touch-icon.png
```

Upload **all** of them, keeping them in the same folder together.

---

## Why it needs a web address at all

iOS will not add a home-screen icon for a file opened from the Files app — the
"Add to Home Screen" option only exists for a real `http(s)://` page. It also
requires **https** for the offline caching to work.

That sounds like a hurdle but it's a one-off, and it costs nothing. Note what does
*and doesn't* get published: the file you upload contains the **questions and the
blank tool**. Every answer, score and note stays in storage on the device that typed
it and is never sent anywhere — there is no server side to this app. So a plain
public URL exposes your question set, not anybody's assessment results.

---

## Option A — your own domain (probably easiest for you)

You already have `brooklynsolutions.ai`. Upload this folder to it via whatever you
normally use (cPanel file manager, FTP, your web person) as something like `/cvm/`,
then visit `https://brooklynsolutions.ai/cvm/` on the phone.

Nothing to configure — it's static files, no database, no PHP, no build step.

## Option B — Netlify (free, about two minutes)

1. On the Mac, go to <https://app.netlify.com/drop>
2. Sign up / log in with a free account
3. Drag this whole folder onto the drop area
4. You get a URL like `https://cheerful-otter-123456.netlify.app` — that's live https
   immediately. You can rename it in Site settings, or point your own domain at it.

## Option C — GitHub Pages (free, permanent)

1. Create a free account at <https://github.com>, then a new **public** repository
2. **Add file → Upload files**, drag all the files in, Commit
3. **Settings → Pages →** Source: *Deploy from a branch*, Branch: `main`, folder `/ (root)`, Save
4. After a minute it's at `https://<your-username>.github.io/<repo-name>/`

---

## Then, on the iPhone or iPad

(On Windows, macOS or Android, use the table at the top — none of this Safari business
applies to you.)

1. Open the URL in **Safari** — it must be Safari; Chrome and Firefox on iOS cannot
   install to the home screen
2. Tap the **Share** button (the square with the arrow)
3. Scroll down, tap **Add to Home Screen**, then **Add**
4. Close Safari and launch it from the new icon

It opens full screen. Put the phone in airplane mode and launch it again to prove the
offline caching worked — everything should still be there.

Repeat those three steps on any other device that needs it. Each device keeps its own
separate set of answers.

---

## Living with it

**Your answers are stored on the device**, in the app's own storage. They survive
closing the app, restarting the phone, and being offline. What they do *not* survive
is deleting the home-screen icon, or "Clear History and Website Data" in Safari
settings. So: **Setup → Export JSON backup** at the end of a session, and AirDrop or
email it to yourself. Restoring is one tap in the same screen.

Because it's installed to the home screen rather than sitting in a Safari tab, iOS
does not apply its 7-day storage eviction to it — an installed web app keeps its data
indefinitely. Export anyway; assessments are worth more than the two seconds it costs.

**The charts** on the Dashboard tab mirror the workbook: a radar (spider) profile and
a grouped bar of Self versus External, with the Assessment Area slicer above them and a
**By area / By subject** toggle for the workbook's two pivot levels. Tap any point or
bar for its figures; "Show table view" gives the same numbers as text. On a phone the
radar labels its spokes by number with a key underneath — seven names crowding the edge
of a 390px screen is unreadable, so the numbers keep the plot big. Radar needs 14 points
or fewer to stay legible, so at subject level pick a single area; the bar chart always
plots every category.

**Exports** use the iOS share sheet, so a CSV can go straight into Mail, Files, or
AirDrop across to the Mac to open in Excel. The columns match the comparison
workbook, so it drops back into your existing analysis.

**Updating the app later**: replace `index.html` on the host, and change the version
string near the top of that edition's `sw.js` (the root one currently reads
`const CACHE = "cvm-assess-v27"` — make it `"cvm-assess-v28"`, and so on). The Self,
External and Consolidator repositories each carry their own.
That version bump is what tells installed devices to pull the new copy — without it
they will keep happily serving the old cached one. Answers already on a device are
untouched by an update.

---

## If you just want a quick look, no hosting

AirDrop `index.html` to the phone and tap it in Files. It opens and runs, so you can
click around and see the thing. But there's no icon, no full screen, and storage in
that preview context is not reliable — don't conduct a real assessment that way.

---

## The three editions

This folder holds the **comparison** edition at the top level, plus `self/` and
`external/` — assessor editions with the lane locked so a submission cannot be filed
against the wrong assessment. Upload all of it and you get three URLs from one site:

```
/            comparison — yours
/self/       the client's people
/external/   external reviewers
```

Each installs as its own app with its own icon, cache and stored answers. Full detail in
DISTRIBUTING.md.

## Which version to use

Two, and they are the same assessment:

| | Best for |
|---|---|
| **This folder (hosted)** | Everyone. A URL you send, installable on any phone, laptop or tablet, and the only route that receives updates |
| **Single `.html` file** | A copy that has to work with no URL at all — email it, keep it on a laptop. It never updates itself |

The hosted route is the one to use. There is no native app: the assessment is a web app,
which is why one upload reaches every device and every platform at once.
