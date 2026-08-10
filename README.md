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

Setting next week's number is step 5, in section 4: type one weekly target, hit *Distribute*, adjust,
then **Commit & lock**.

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
index.html    the whole application
README.md     this file
```

That is the entire repo, and it should stay that way. To update, replace `index.html` and push; GitHub
Pages redeploys in a minute or two. Anyone with the page already open should reload.
