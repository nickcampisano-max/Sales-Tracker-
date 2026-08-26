# CKC / ATP Weekly Sales Tracker

A single self-contained HTML page that turns three Toast exports into the weekly sales tracker.
No server, no build step, no dependencies, no network requests. Open it and it runs.

Replaces the manual `CKC_ATP Weekly Projections & Sale Tracker.xlsx`.

---

## The weekly routine

Four steps. Takes about a minute.

1. **Open workbook** → pick the latest `.json` from the shared Drive folder.
   If the page says *"No history loaded"*, this step has not been done yet — don't skip it, or you'll
   save a file containing only this week and lose the history.
2. **Drop the three Toast exports** onto the page. All three at once is fine.
3. **Check the preview, then write them in.** Anything that overwrites a number already stored is
   reported before and after.
4. **Save workbook** → put the file back in the shared Drive folder.
   **Export for Drive** gives you the `.xlsx` that people actually read.

Setting next week's number is step 5, in section 4. The weekly target box arrives **pre-filled with a
suggestion and the arithmetic behind it** — last year's same week scaled by how recent weeks are tracking
against their own year-ago weeks, or a trailing average when there's no usable week from last year.
It is a suggestion. Type over it, hit *Distribute*, adjust any day directly, then **Commit & lock**.

### Starting from scratch, or rebuilding

You don't need a `.json` to begin. **Select every Toast CSV you have and drop them all at once.** Files are
merged by bucket and each day is routed to its own week, so a year of exports in one drop rebuilds the
whole history. Then *Save workbook* and that file becomes the record from then on.

This is also the recovery path if a workbook is ever lost or corrupted — the CSVs are the source of truth,
the `.json` is just a convenience.

---

## Handing over to someone else

The whole history is one `.json`. That file is the only thing that changes hands.

Do it at a period boundary so the new person starts on week 1 of a fresh period.

**Outgoing person, in order:**

1. Import the final week of the period and check no day is marked *partial*. A gap left now is inherited
   permanently — go pull the missing bucket first.
2. Set and **Commit & lock** the projection for the incoming person's first week, so their first job is
   purely importing actuals.
3. **Save workbook.** Confirm the red "not saved" bar is gone. This file is the handoff.
4. Put it in the shared Drive folder.
5. **Settings → This browser is for → Review only.** Do this before they start. Two machines set to
   *Inputting* is the one way to lose a week's work.

**Incoming person, once:**

1. Open the URL and bookmark it.
2. **Settings → Inputting**, and put your name in. Every save is stamped with it from then on.
3. **Open workbook** → the `.json` from Drive. Check the week count looks right.
4. Do one supervised run end to end before you're on your own.

**From then on**

The incoming person's browser is the live copy. The outgoing person's goes stale the moment the first new
week is saved, silently — so re-open the latest file from Drive whenever you want current numbers, rather
than trusting what's on screen.

If you ever open a workbook that is *older* than what your browser already holds, you'll be warned and told
both revision numbers. Read it before clicking through: it means someone else has saved more recently than
the file you picked.

---

## The three exports

Toast → **Sales by day**, one export per bucket. The time window is applied in Toast *before* exporting.

| Export | Bucket | Window |
|---|---|---|
| CKC Lunch | CKC AM | 10am – 3pm |
| CKC Dinner | CKC PM | 3pm – close |
| ATP | ATP | 5pm – close |

All three come out with the same columns: `yyyyMMdd, Net sales, Total orders, Total guests`.

**Pull the same date range for all three.** If the ranges differ, the days covered by only some of them
come in partial, and the weekly total will read low until you pull the rest.

Date ranges don't need to start on a Monday and can span any number of weeks — each day is routed to
its own week automatically. Pull twelve months at once to backfill.

The page identifies each file from its name. Keep **Lunch**, **Dinner** and **ATP** in the filenames and
it is automatic. A file it can't identify is flagged rather than filed somewhere plausible — set the
bucket yourself from the dropdown before writing it in.

---

## Where the data lives

**In the Drive folder, not in this repo.** The `.json` workbook is the record.

- Every save is a full snapshot of *every* week, so there is one file and opening it restores everything.
- Drive keeps prior versions of that file, which is the rollback if a bad import lands.
- The browser also keeps a working copy so a crash or a refresh doesn't lose the day. It is a buffer only —
  it does not survive clearing the browser or moving to another machine. **The file in Drive wins.**

> **Never commit sales data to this repo.** GitHub Pages sites are public even when the repository is
> private. No `.json`, no Toast CSVs. Code only.

---

## Reading the numbers

**Variance is `Actual − Projection`.** Green and positive is a beat. The old spreadsheet computed it the
other way round, so a negative number there actually meant a beat — the signs are opposite between the two.

**A committed projection is frozen.** Editing after commit is logged as a revision and the headline
variance still measures against the original, so the baseline can't drift.

**Comparisons only use matched days.** A part week is compared against the same days in the reference
period, never against a finished week. The range is always stated on screen.

**Some numbers are deliberately withheld** rather than shown. If you see *"withheld"*, the tool has decided
the comparison would be misleading and says why. The usual causes:

- the reference week is missing days
- a venue was closed for the whole reference period, so the gap is a closure rather than trading
- a service pattern changed between the two periods

This is the point of the tool. A closure compared against a normal week produces a large, confident,
completely fake percentage. Those are withheld instead of printed.

**Blank vs closed vs zero:**

| Shown | Means |
|---|---|
| blank | no export has covered that day yet — hover the cell for which case it is |
| `closed` | the export covered that day and Toast reported nothing |
| `0` | genuinely zero |

---

## The two views

The page opens on four sections: import, the week, projection vs actual, next week. That's the weekly job.

The **Analysis** button in the header reveals the rest — week / day / period comparisons against prior week,
prior period, trailing 13 weeks and same week last year; the period roll-up; covers; and full week history.
Nothing is deleted, it just isn't in the way. The choice is remembered.

---

## Known caveats

**Order and guest counts are not verified.** The Toast account is Clever Koi with ATP as a revenue centre,
so a count may cover the whole account rather than the one bucket. Net sales *are* confirmed correctly
scoped — the three buckets sum exactly to the day totals from the original spreadsheet. Nothing in the
tool's maths uses the counts; the covers panel is marked indicative.

**The fiscal calendar is inferred, not published.** Period and week labels are derived from an anchor set
in Settings, worked out from the labels in the original spreadsheet. Confirm it against the official
calendar before relying on a period label.

**The Excel export is written from scratch** — no library — so it stays dependency-free on a public page.
It has been validated as a well-formed workbook, but check it opens cleanly in your Excel or Sheets before
circulating one.

---

## Repo contents

```
index.html      the whole application
CHANGELOG.md    this file
```

That is the entire repo, and it should stay that way. To update, replace `index.html` and push; GitHub
Pages redeploys in a minute or two. Anyone with the page already open should reload.

---

# Change history

Newest first. Dates are when the work landed, not when it was deployed.

## 2026-08-09 — cut down, suggested targets, drop-the-folder

**Simplified**

- Default view reduced from nine sections to four: import, the week, projection vs actual, next week.
- Everything else moved behind a single **Analysis** toggle. Nothing removed; hidden panels don't render
  at all until opened. The choice persists.
- KPI tiles trimmed from seven to four.
- **Loud empty state.** An unloaded workbook and a genuine Toast data gap previously looked identical —
  both just blank cells. The page now says which it is, and blank cells carry a tooltip that distinguishes
  *"nothing loaded yet"* from *"no export has covered this day"*.
- Week and actuals panels merged into one.

**Suggested weekly target**

- The target box now pre-fills with a suggested number and states how it was derived. Previously the
  distributor produced the day-by-day *shape* but the weekly total had to be typed from nothing.
- Preference order: last year's same week scaled by the trailing year-over-year ratio, then flat to last
  year, then a trailing average — each labelled so the basis is never hidden.
- Same guards as everywhere else: a year-ago week that is incomplete, or had a venue dark, is refused as
  a basis rather than quietly used.
- Prefill never overwrites a typed value, and every day can still be overridden by hand afterwards.

**Import fixes** — all three found by dropping the entire CSV folder at once

- A file whose name identifies no bucket is now sized against the *other files in the same drop*, not just
  against stored history. Previously, dropping a full folder onto an empty page filed the one
  ambiguously-named export into the wrong bucket. Resolution happens after the whole batch has loaded, so
  file arrival order doesn't matter.
- Dates covered by an export but returning no row are now recorded as closed. Previously they were skipped
  entirely, which left holiday weeks permanently and wrongly marked incomplete.
- Export coverage is now the span of dates the export *asked* for, not just the dates that carried an
  amount. Rows present with a blank amount used to truncate the range and make trading days look uncovered.
- Staged files re-preview after a workbook is loaded, instead of showing week labels computed against the
  previous, often empty, history.

**Found in the original spreadsheet**

- One week is overstated: a single transaction was entered into two different buckets *and* into the typed
  week total, so it counted more than once and was allocated to the wrong dayparts. The exports disagree
  with that week's stated total, and the exports are correct. That week had been read as a large beat
  against projection; part of the beat wasn't real.

## 2026-08 — comparisons and their guards

- Added week, day and period comparisons against prior week, prior period, trailing 13-week average and
  same week last year, on screen and in the Excel export.
- **Period roll-up** — whole period against whole period, which week-4-weeks-ago is not. A period in
  progress compares matched week positions only.
- Guards added as artifacts were found, each one from a real number the tool had printed:
  - a closure a year ago read as a large gain → withheld when a venue was dark for the whole reference
  - a single lost day produced a +425% day comparison → day-level withholding
  - one dark week averaged with live ones hid a closure inside a period → per-constituent-week checking,
    with a threshold so a long average tolerates one bad week and a single-week reference does not
  - a service change mid-basis skewed the day mix → flagged when a weekday was dark in part of the basis
- Fixed: both file controls now accept either a workbook or Toast CSVs, so handing the workbook button a
  CSV imports it instead of erroring.
- Fixed: files whose names don't identify a bucket are matched by sales level and flagged, never filed
  silently. A file that can't be identified either way stops and asks.

## 2026-07-31 — history backfill

- Twelve months of Toast exports loaded: 62 weeks, 60 complete.
- Seed data removed from the page so nothing sensitive is ever committed. History moved to a workbook
  `.json` kept in Drive.
- Week navigation rebuilt to handle a year — selector plus a clickable recent-weeks strip instead of tabs.
- Day-of-week projection distributor: one weekly target, split by the trailing mix from complete weeks.
- Dependency-free `.xlsx` writer (ZIP + OOXML, no library) so the page makes no network requests.
- Autosave after every import, plus an unsaved-changes warning on close.

## 2026-07-30 — first build

- Read the original spreadsheet and reproduced its logic; all five transcribed weeks reconcile to the cent.
- Import rebuilt around the three day-part exports after the real Toast format turned out to have no
  revenue-centre or time columns.
- Partial days left blank rather than zeroed, so an unpulled report can't drag a weekly total down.
- Projections freeze on commit; post-commit edits logged as revisions.
- Comparison range made an explicit control.
- Variance sign flipped to `Actual − Projection`.

### Issues found in the original spreadsheet

- A week's forecast had been copy-pasted forward and then edited, so variance was measured against a
  baseline that moved between sheets.
- The "Difference" cell meant a different day range on three different sheets while looking identical.
- Its sign convention made a beat look like a shortfall.
- One week's roll-forward was skipped entirely, so the next period was never forecast.
- A Sunday recorded as lunch-only turned out to be a genuine Toast gap, confirmed against the raw exports —
  dinner and ATP both missing for that date. Worth raising with Toast; the revenue may be recoverable.
