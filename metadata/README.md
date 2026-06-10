# `metadata/`

- `knmi_station_sensor_inventory.csv` — every KNMI sensor (`LOCATION`) seen in `data/ToSend-01-20-2025/`, with name, coordinates, ground altitude, and per-variable coverage counts. Built by chunk `build-knmi-sensor-inventory` in `code_import_combine_clean_data/obs_import_and_visualisation.Rmd` (samples 4 mid-month days × 4 years = 16 days, 2021–2024).
- `WOW_data.txt` — schema for the WOW (crowd-sourced) station data.

## `knmi_station_sensor_inventory.csv` columns

| Column | Meaning |
|---|---|
| `station_id` | KNMI station ID, prefix before first `_` in `LOCATION` (e.g. `240` = Schiphol/Amsterdam, `344` = Rotterdam) |
| `LOCATION`, `NAME` | sensor ID and human-readable name |
| `LATITUDE`, `LONGITUDE`, `ALTITUDE` | as written in the raw file; `ALTITUDE` is **ground** elevation (m AMSL), **not** sensor mounting height |
| `sample_dates`, `n_sample_dates` | which sampled days the sensor appeared in (max 16) |
| `total_obs`, `total_T_DRYB`, `total_T_DEWP`, `total_U_10` | non-NA counts across sampled days (ceiling = 16 × 144 = 2304) |
| `T_mean_sampled` | mean of daily mean `T_DRYB_10` across sampled days (4 seasons → close to annual mean, sanity check only) |

## Caveats

- **No per-sensor mounting height anywhere** in the raw KNMI data. `T_DRYB_10` / `T_DEWP_10` / `U_10` are at the WMO/KNMI standard 1.5 m screen height by convention, not from any field.
- **`_10` = 10-minute averaging, not 10 metres.**
- One station ID can group many physical sensors at different locations (e.g. `240` has 7: Schiphol runways + 4 satellite towns; only Muiden reports its own coordinates).

## Regeneration

Open the chunk `build-knmi-sensor-inventory` and run it manually (`eval = FALSE`). Edit `sample_dates` to change the sampling.
