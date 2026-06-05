# EvacGap

> Measuring — and explaining — how long California wildfire evacuation alerts take to reach each community.

**WiDS Datathon 2026 · Track 1: Equitable Evacuations**

📊 [Slide deck](FILL_IN) · 📝 [Full writeup](FILL_IN) · 🎥 [Demo video (≤3 min)](FILL_IN) · 🏆 [Kaggle submission](FILL_IN)

---

## The question

When a wildfire breaks out, evacuation orders go out zone by zone, and the minutes between a fire starting and a zone being warned can decide whether people get out safely. EvacGap asks: **after a California wildfire is first reported, how long does each zone wait for its first evacuation alert — and does that wait fall disproportionately on more vulnerable communities?**

## Key findings

**1. Evacuation waits are extremely uneven.** Most fires get an alert quickly, but a long right tail of cases waits hours — or, in the worst cases, days. We reason about the full distribution rather than an average, because a few severe delays hide behind a single mean.

images/image.png

**2. Vulnerability turns delay into an equity question.** Joining the delay to ACS county demographics (poverty, child poverty, unemployment, transit access, income) shows delay isn't only about the fire — it's also about whether a community has the resources to act on an alert.

**3. Four counties carry both high delay and high vulnerability.** Splitting counties at the median of each axis, four land in the high-lag *and* high-vulnerability quadrant:

| County | Fires | Median lag | Vulnerability |
|---|---:|---:|---:|
| Kern | 7 | 100 min | 0.81 |
| Riverside | 18 | 99 min | 0.60 |
| Fresno | 10 | 83 min | 0.85 |
| Butte | 6 | 56 min | 0.61 |

![Delay vs. vulnerability quadrant](figures/equity_quadrant.png)

We translate each into targeted recommendations (bilingual alerts, transport support, outreach, earlier trigger review), and estimate that **~6,857 minutes (~114 hours)** of lead time could be recovered across the top-20 priority fire-zones with earlier alert triggers — a model-based estimate of potential, not a guarantee.

## Data

EvacGap uses **WatchDuty wildfire incident data** (from the WiDS Datathon 2026 dataset) plus **U.S. Census ACS 2017 county data**. The required files are:

- `geo_events_geoevent.csv`, `geo_events_geoeventchangelog.csv` — fire records and status history
- `evac_zones_gis_evaczone.csv`, `evac_zones_gis_evaczonechangelog.csv` — evacuation zones and status changes
- `evac_zone_status_geo_event_map.csv` — the zone-to-fire bridge
- `fire_perimeters_gis_fireperimeter.csv` — fire perimeters (for size)
- `acs2017_county_data.csv` — county demographics

The raw data is **not** committed to this repo (size and usage terms). See [`data/README.md`](data/README.md) for download links and where to place each file.

## Running it

```bash
git clone FILL_IN
cd evacgap
pip install -r requirements.txt
```

1. Download the data (see `data/README.md`) and place the CSVs under `data/`.
2. Open `notebooks/evacgap.ipynb` and set the input path near the top to your data folder:
   ```python
   input_root = Path("data")   # or "/kaggle/input" if running on Kaggle
   ```
3. Run the notebook top to bottom. It reproduces the full pipeline, the figures in `figures/`, and the result tables in `outputs/`.

> **Note on the analysis window:** this repo uses the **0–3 day** evacuation-lag filter (1,266 zone rows across 210 fires), matching the deck and writeup.

## Repository structure

```
evacgap/
├── README.md
├── requirements.txt
├── notebooks/
│   └── evacgap.ipynb              # full analysis, runnable top-to-bottom
├── figures/
│   ├── lag_distribution.png
│   ├── lag_vs_vulnerability.png
│   └── equity_quadrant.png
├── outputs/
│   ├── county_equity.csv          # per-county lag + vulnerability
│   └── priority_recommendations.csv
└── data/
    └── README.md                  # where to get the data (not the data itself)
```

## Method, in brief

Evacuation lag is the minutes from a fire's first log to the first evacuation-status change for a zone linked to it. Because no table connects zones to fires directly, we chain three joins (zone → `uid_v2` → `geo_event_id` → fire). We parse the JSON status column, sort chronologically before taking the first event per zone, and keep only lags of 0–3 days (removing backfilled negatives and reused-zone-ID artifacts). We then restrict to California, attach each county's ACS profile, and build a composite vulnerability score to surface where delay and need overlap. Full detail is in the [writeup](FILL_IN).

## Limitations & next steps

County-level demographics average away within-county variation; the ACS data is from 2017; the analysis is California-only; and the recoverable-lead-time figure is a model estimate. Next: validate earlier triggers against fire-spread signals, move to tract-level granularity, and extend beyond California.

## Team

Qifei & Alex
