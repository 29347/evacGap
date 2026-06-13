# EvacGap — Measuring Evacuation-Alert Delay Across California Wildfires

**WiDS Datathon 2026 · University Challenge**
**Prize Track:** Route 1 — Accelerating Equitable Evacuations

> EvacGap measures how long California communities wait for wildfire evacuation alerts, and shows that the longest delays fall disproportionately on the communities least equipped to respond quickly.

**WiDS Datathon 2026 · Track 1: Equitable Evacuations**

🔗 [GitHub repo](https://github.com/29347/evacGap) · 📊 [Slide deck](https://docs.google.com/presentation/d/1TjxNMsREkufSimdF5__-xwLaS-16bsjp/edit?usp=sharing&ouid=105165258898800248652&rtpof=true&sd=true) · 🎥 [Demo video (≤3 min)](https://www.youtube.com/watch?v=OkAYfCSZIFY)

---

## Project & Team Info

| Field | Value |
|---|---|
| **Project Title** | EvacGap — Measuring Evacuation-Alert Delay Across California Wildfires |
| **Team Name** | _EvacGap |
| **University** | Pasadena City College |
| **Course** | Independent Study CS20_ |
| **Term** | Spring 2026_ |

**Team Members**
- Thant Kyi Thu (GitHub: @thantkyithu)
- Qifei Li (GitHub: @qifeili84)

---

## Chosen Route

**Route 1: Accelerating Equitable Evacuations**

**Core question:** Do the longest waits for evacuation alerts fall on the communities least equipped to respond quickly — and where could earlier triggering recover the most lead time?

EvacGap measures evacuation-alert delay across California wildfires: the time from when a fire is first logged to when each evacuation zone receives its first alert, using WatchDuty incident data. We then join that delay to U.S. Census demographics to ask an equity question, and translate the findings into targeted, county-level interventions.

---

## Dataset Overview

**WatchDuty wildfire incident data (CSV)**
Real-time incident and alert records for California wildfires. We use the incident logging timestamps and the per-zone evacuation-alert timestamps to compute alert lag (time from a fire first being logged to a zone's first evacuation alert).

**ACS 2017 Census demographics**
County- and zone-level social vulnerability indicators (used to test whether longer delays concentrate on more vulnerable populations).

**Processing and key assumptions**
- The dataset is connected through a three-join chain: evacuation zone IDs → `uid_v2` → fire IDs. This is what lets each zone-level alert be tied back to a specific fire and then to demographics.
- We did **not** apply the `is_visible` filter to WatchDuty records — it removes nearly all historical data and would collapse the analysis.
- Outlier lag filtering: we drop negative lags (backfilled records, where an alert is logged as preceding the fire) and very large lags (cross-season reuse of the same zone). Both are data-quality artifacts rather than real evacuation delays.
- Analysis uses a **3-day evacuation-lag window** to focus on alerts tied to an active incident.

---

## Approach

**Preprocessing and cleaning**
- All timestamps parsed with `utc=True` so that lag arithmetic is correct across time zones and daylight-saving boundaries.
- Working frames created with `.copy()` to avoid chained-assignment side effects when filtering.
- Records sorted chronologically before `groupby(...).first()` so that "first alert per zone" reliably returns the earliest timestamp, not an arbitrary row.

**Lag computation**
- For each evacuation zone, alert lag = (first evacuation-alert time) − (time the fire was first logged), per fire.
- Negative and extreme lags removed as described above; a 3-day window applied.

**Equity analysis**
- Per-zone and per-county lag joined to ACS 2017 demographics.
- Counties ranked by the intersection of high alert delay and high social vulnerability.

**Recoverable-lead-time estimate**
- For the highest-priority fire-zones, we estimate how much evacuation lead time earlier alert triggering could have recovered.

**Charting note**
- Distribution charts use filtering (dropping out-of-range lags) rather than clipping, so that bars reflect real counts instead of values squeezed to an axis boundary.

**Tools:** Python, pandas, matplotlib .

This is a measurement-and-equity analysis rather than a predictive-modeling task, so the results below are descriptive findings rather than model metrics (AUC, F1, RMSE).

---

## Results

- **Scope (after cleaning, 3-day window):** 1,266 zone-level alerts across 210 California fires.
- **Delay is highly uneven** across zones and counties.
- **Four priority counties** — Kern, Riverside, Fresno, and Butte — sit at the intersection of high alert delay and high social vulnerability.
- **Recoverable lead time:** across the top-20 priority fire-zones, earlier alert triggering could have recovered roughly **6,857 minutes (~114 hours)** of evacuation lead time.
- **Danger-signal gap:** 56% of fires showed danger signals in the data before any evacuation order was issued. _(Verify against the full 210-fire dataset before final grading.)_

**Targeted interventions (per priority county):** bilingual alerts, transport assistance, community outreach, and earlier alert triggers.

**Limitations and ethical considerations**
- WatchDuty timestamps reflect when information was logged, which may differ from the real-world moment an order was issued.
- ACS 2017 demographics may not reflect current population distribution.
- Outlier removal is a defensible data-quality choice but does discard a small number of edge-case records; the rationale is documented above.

---

## Team Contributions

| Member | Contributions |
|---|---|
| Thant Kyi Thu & Qifei Li | Presentation, Kaggle narrative writeup, repository structure, equity analysis, app mockup Kaggle notebook, data pipeline, lag computation, figure export |

---

## How to Reproduce

```bash
git clone FILL_IN_GITHUB_REPO_URL
cd evacgap
```

1. Open the notebook in Google Colab (badge at the top of this README).
2. Run the cells top to bottom. The notebook auto-detects whether it is running in Colab or locally and resolves the data paths accordingly.
3. The three exported figures are written out at the end of the run.

---

## Questions?

Visit the WiDS community hub.
