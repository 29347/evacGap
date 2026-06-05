# Data

These files are required to run the notebook. They are **not** committed to the repo
(size + usage terms) — download them and drop them into this `data/` folder.

## Required files
- `geo_events_geoevent.csv`
- `geo_events_geoeventchangelog.csv`
- `evac_zones_gis_evaczone.csv`
- `evac_zones_gis_evaczonechangelog.csv`
- `evac_zone_status_geo_event_map.csv`
- `fire_perimeters_gis_fireperimeter.csv`
- `acs2017_county_data.csv`

## Where to get them
- **WatchDuty wildfire incident files** — from the WiDS Datathon 2026 competition dataset on Kaggle: <FILL_IN>
- **`acs2017_county_data.csv`** (U.S. Census ACS 2017 county demographics) — public Kaggle dataset: <FILL_IN>

## Placing them
Drop the files anywhere inside this `data/` folder. The notebook finds them by name
(it searches recursively), so subfolders are fine. Then set the input path near the top
of the notebook:

```python
input_root = Path("data")   # or "/kaggle/input" when running on Kaggle
```
