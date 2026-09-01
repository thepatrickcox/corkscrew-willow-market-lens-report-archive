# The Market Lens — Archive

The running record of a weekly market-intelligence instrument published by [Corkscrew Willow Advisory](https://corkscrewwillowadvisory.com). Each Friday's report is archived here as published, alongside a machine-readable series of the readings and an append-only log of every dated call the reports have made.

**This is not investment advice.** It is a published record of observations, with sources named and confidence stated. Nothing here describes positions or holdings.

---

## Why this repository exists

The Lens is published as a single live page that is replaced each week. That is right for a reader — it should always show the current reading — and wrong for research, because the series is where the value accumulates. A single week's Buffer is colour. Fifty-two weeks of Buffer, recorded with sources and never quietly revised, is a dataset.

Three things follow from putting it under version control rather than in a folder:

**Revisions become visible.** Every edit is timestamped and diffable. If a published reading is ever changed, the change is in the history, permanently, whether or not anyone is looking. That converts an editorial promise into infrastructure.

**The calls can be graded.** The reports make dated, falsifiable claims. [`triggers/trigger_log.md`](triggers/trigger_log.md) collects every one with the criterion that would mark it wrong, and grades them as the dates arrive — including the ones that fail.

**The readings become usable.** [`data/weekly_readings.csv`](data/weekly_readings.csv) is one row per Friday, in the same one-fact-per-cell form as the author's [historical cycles research](https://github.com/thepatrickcox/history-cycles-crashes-correlation), so the two can eventually be joined.

---

## What is here

```
reports/                weekly HTML reports, one per Friday, as published
data/
  weekly_readings.csv   one row per Friday — the series
  data_dictionary.md    every column defined, with its source
triggers/
  trigger_log.md        pre-committed calls, dates, and grades
companions/             standing instruments published alongside the weekly
```

**Companion instruments.** *The Borrower's Lens* grades household debt health by product and income class. *The World Lens* tracks the major economies, sorting market-priced data from state-reported data. *The Distress Watch* is a weekly Greenville County foreclosure instrument. *The Bond Hurdle* is a free teaching page on when a long government bond becomes worth owning. Each carries its own cadence and its own "as of" stamps.

---

## The rules the reports run on

These are the editorial constraints the published work is held to. They are listed here so a reader can check whether they were kept.

**Fetch or omit — never estimate.** Every figure is retrieved live in the session that produces the report, with a named source and date. A figure that cannot be retrieved is marked *not confirmed this run* and left out. A labelled guess is still a guess.

**Conflicting sources are resolved, not averaged.** The primary and newest source wins, and the discarded reading is named. One recurring example is documented in the reports: a widely-syndicated 200-day moving average for the S&P 500 that has been inconsistent with the index's own trading range for weeks is discarded every run in favour of a corroborated source, and the discard is stated each time.

**One gauge, one home.** Each instrument is owned by exactly one section and referenced by pointer elsewhere. Duplication across documents was found and removed in August 2026; the merge is in the commit history.

**Every forward claim carries a kill switch.** No forward-looking statement is published without a named future release or date that would mark it wrong. Those live in the trigger log.

**Corrections are published, not absorbed.** Two are already on the record — an overstated national inventory turn, and a set of board rows published carrying stale values because of a scripting error. Both are in the trigger log under *Corrections*.

---

## Reading the data file

Figures marked `~` in the reports are approximations pending a pipeline refresh; the CSV carries the number without the tilde and the `notes` column records the qualification. Blank cells mean the figure was not fetched to a verified value that week — they are not zeros and should not be interpolated. Slow-moving gauges (the cyclically-adjusted earnings multiple, the market-cap-to-GDP ratio) are scaled to the week's close pending a full recomputation and should be treated as directionally reliable and precisely approximate.

The 200-day moving average column deserves particular care. It is a slow series and the source used publishes on a lag; where a stale print is carried forward, the computed Buffer is modestly overstated. This is stated in each report and is visible in the CSV as a repeated `dma200` value across consecutive weeks.

---

## Licence and citation

Documents and data are released under **CC BY 4.0** — reuse freely with attribution. If you find an error in the factual record, or have primary-source data that extends or challenges anything here, open an issue. The methodology depends on counter-examples being included.

*Archive begins 2026-07-31. Maintained by Patrick Cox.*
