# Data Dictionary — `weekly_readings.csv`

One row per Friday, anchored to that week's close. Blank means **not fetched to a verified figure that week** — it is not a zero and must not be interpolated.

| Column | Type | Definition | Source |
|---|---|---|---|
| `week_ending` | date (ISO) | The Friday the report is anchored to. All market figures are that day's close unless a note says otherwise. | — |
| `sp500_close` | number | S&P 500 index closing level. | CNBC / Reuters / Yahoo Finance |
| `dma200` | number | S&P 500 200-day simple moving average. See caution below. | Financhill (`$SPX`), cross-checked against the index's own trading range |
| `buffer_pct` | percent | `(sp500_close ÷ dma200 − 1) × 100`. The instrument the reports lead with. | Computed |
| `pct_from_record` | percent | Distance from the highest closing level recorded to date. Negative is below. | Computed |
| `cape` | number | Cyclically-adjusted price/earnings ratio (Shiller). Scaled to the week's close pending full recomputation — directionally reliable, precisely approximate. | multpl / GuruFocus / YCharts |
| `buffett_pct` | percent | Total US market capitalisation ÷ GDP. Same scaling caveat as `cape`. | GuruFocus |
| `vix` | number | CBOE volatility index close. | CBOE / Yahoo |
| `hy_oas_pct` | percent | ICE BofA US High Yield option-adjusted spread. Frequently carried from a prior week — check `notes`. | FRED `BAMLH0A0HYM2` |
| `ust_2y`, `ust_10y`, `ust_30y` | percent | Treasury constant-maturity yields. | FRED `DGS2` / `DGS10` / `DGS30`, CNBC |
| `eurusd` | number | Euro against the US dollar. Added from the week ending 2026-09-04. | Market data |
| `dxy` | number | US Dollar Index. | Trading Economics |
| `mortgage_30y` | percent | 30-year fixed average, Freddie Mac Primary Mortgage Market Survey. Published Thursdays, so it reflects a survey window closing before Friday. | Freddie Mac PMMS |
| `mmf_assets_tn` | number ($tn) | Total money-market fund assets. Published Wednesdays for the week ending the prior Wednesday — typically lags the row date by about a week. | ICI |
| `cpi_yoy` | percent | Consumer Price Index, year-over-year. Monthly — populated only in the week of release. | BLS |
| `core_pce_yoy` | percent | Core personal consumption expenditures price index, year-over-year. The Federal Reserve's preferred gauge. Monthly. | BEA |
| `payrolls_change` | integer | Non-farm payroll change for the reference month. Monthly. Subject to substantial revision. | BLS |
| `retail_sales_mom` | percent | Advance retail and food services sales, month-over-month. Monthly. Carries a sampling error of roughly ±0.4 percentage points. | US Census Bureau |
| `sentiment_umich` | number | University of Michigan index of consumer sentiment. Preliminary unless `notes` says final. | University of Michigan Surveys of Consumers |
| `trigger_state` | enum | `WAITING` (index above its 200-day line), `WATCH` (below for a first Friday), `TRIGGERED` (below for two consecutive Fridays), `RECOVERED` (back above after a trigger). | Computed from `buffer_pct` |
| `verdict` | text | The report's one-line status for that week, as published. | — |
| `notes` | text | Qualifications, records set, and anything material that has no column. | — |

---

## Cautions

**`dma200` is the weakest series here.** The source publishes on a lag, so a stale print is sometimes carried forward — visible as an identical value across consecutive weeks (2026-08-14 through 2026-08-28, all 7076.24). Where that happens the computed `buffer_pct` is modestly **overstated**, because the moving average has continued to rise beneath a flat index. Each affected report says so in its sources block.

A second, widely-syndicated source for this series has been discarded every week since the archive began, because its readings are inconsistent with the index's own trading range over the same 200 sessions — a 200-day average cannot rise several hundred points in nine trading days. The discard is documented in each report rather than made silently.

**Monthly series are sparse by design.** `cpi_yoy`, `core_pce_yoy`, `payrolls_change`, `retail_sales_mom` and `sentiment_umich` populate only in the week the figure is released. Do not forward-fill them; the blank carries information about publication timing.

**`hy_oas_pct` repeats.** The spread has been carried from a last-confirmed reading across several weeks. Treat repeated values as "not refreshed" rather than "unchanged," and consult `notes`.

**Revisions are not back-written.** `payrolls_change` in particular is heavily revised — the July 2026 figure was published alongside downward revisions of 103,000 to the prior two months. Rows record what was known that week. Later revisions are noted in the trigger log rather than silently applied, so the file preserves what the reports were actually working from.
