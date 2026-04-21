# Get Records

**You need a special authorization to use this endpoint, ask it to contact@nolio.io**

**This endpoint has an additional rate limit of 20 requests per minute.**

## Endpoint:

- **/get/records/**
- HTTP Method: **GET**

## Parameters

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `cat` | **yes** | string | Record category. One of: `ppr` (power), `phrr` (heart rate), `par` (pace), `ptr` (torque), `pvar` (vertical ascent speed), `pcadr` (cadence) |
| `record_type` | **yes** | string | `time` (best value over a duration) or `distance` (best value over a distance) |
| `from` | no | string | Start date (inclusive), format `YYYY-MM-DD` |
| `to` | no | string | End date (inclusive), format `YYYY-MM-DD` |
| `sports` | no | string | Comma-separated [sport IDs](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#sport-map) (e.g. `9,23`). If omitted, returns the absolute best across all sports |
| `items` | no | string | Comma-separated durations in seconds or distances in meters. If omitted, all available items are returned. See [Available items](#available-items) below |
| `athlete_id` | no | int | For coaches: retrieve records of a managed athlete |

## Available items

### Time-based (`record_type=time`)

Durations in seconds:

| Duration | Value | | Duration | Value | | Duration | Value |
|----------|-------|-|----------|-------|-|----------|-------|
| 1s | `1` | | 10s | `10` | | 5min | `300` |
| 2s | `2` | | 15s | `15` | | 6min | `360` |
| 3s | `3` | | 20s | `20` | | 7min | `420` |
| 4s | `4` | | 25s | `25` | | 8min | `480` |
| 5s | `5` | | 30s | `30` | | 9min | `540` |
| 6s | `6` | | 45s | `45` | | 10min | `600` |
| 7s | `7` | | 1min | `60` | | 12min | `720` |
| 8s | `8` | | 2min | `120` | | 15min | `900` |
| 9s | `9` | | 3min | `180` | | 20min | `1200` |
| | | | 4min | `240` | | 25min | `1500` |

| Duration | Value | | Duration | Value |
|----------|-------|-|----------|-------|
| 30min | `1800` | | 3h | `10800` |
| 45min | `2700` | | 3h30 | `12600` |
| 1h | `3600` | | 4h | `14400` |
| 1h30 | `5400` | | 4h30 | `16200` |
| 2h | `7200` | | 5h | `18000` |
| 2h30 | `9000` | | 5h–10h | `19800`–`36000` |

### Distance-based (`record_type=distance`)

Distances in meters:

| Distance | Value | | Distance | Value |
|----------|-------|-|----------|-------|
| 400m | `400` | | 10km | `10000` |
| 500m | `500` | | 15km | `15000` |
| 800m | `800` | | 10mi | `16090` |
| 1km | `1000` | | 20km | `20000` |
| 1500m | `1500` | | Half-marathon | `21097` |
| 1mi | `1609` | | 25km | `25000` |
| 2km | `2000` | | 30km | `30000` |
| 3km | `3000` | | 35km | `35000` |
| 2mi | `3218` | | 40km | `40000` |
| 5km | `5000` | | Marathon | `42195` |
| 5mi | `8045` | | 50km | `50000` |
| | | | 60km–100km | `60000`–`100000` |
| | | | 50mi | `80450` |
| | | | 100mi | `160900` |

## Examples

```
Power records (cycling), time-based:
https://www.nolio.io/api/get/records/?cat=ppr&sports=9&record_type=time

Power records (cycling), 5min and 20min only:
https://www.nolio.io/api/get/records/?cat=ppr&sports=9&items=300,1200&record_type=time

Heart rate records, all sports:
https://www.nolio.io/api/get/records/?cat=phrr&record_type=time

Pace records (running), time-based (5min, 20min, 60min):
https://www.nolio.io/api/get/records/?cat=par&sports=1&items=300,1200,3600&record_type=time

Pace records (running), distance-based (10km, half-marathon, marathon):
https://www.nolio.io/api/get/records/?cat=par&sports=1&items=10000,21097,42195&record_type=distance

Power records in 2025:
https://www.nolio.io/api/get/records/?cat=ppr&record_type=time&from=2025-01-01&to=2025-12-31

Heart rate records from 2022 to 2024:
https://www.nolio.io/api/get/records/?cat=phrr&record_type=time&from=2022-01-01&to=2024-12-31

Records of a managed athlete (coach access):
https://www.nolio.io/api/get/records/?cat=ppr&record_type=time&athlete_id=456
```

## Returns

A JSON array of record objects, one per item (the absolute best across all sports unless `sports` is specified). Each record contains:

| Field | Type | Description |
|-------|------|-------------|
| `id` | int | Record ID |
| `value` | int or float | Record value. Integer for power (W), heart rate (bpm), torque (Nm), vertical ascent speed (m/h), cadence (rpm). Float for pace (mps) |
| `unit` | string | Unit of the value: `W`, `bpm`, `mps`, `Nm`, `m/h`, `rpm` |
| `date` | string | Date of the record (`YYYY-MM-DD`) |
| `cat` | string | Record category (same as the `cat` parameter) |
| `record_type` | string | `time` or `distance` |
| `inner_val` | int | Duration (seconds) or distance (meters) of the item. *Only present if available* |
| `training_id` | int | ID of the associated training. *Only present if available* |
| `training_name` | string | Name of the associated training. *Only present if available* |
| `sport_id` | int | Sport ID. *Only present if available* |
| `sport_name` | string | Sport name. *Only present if available* |

### Example response

```json
[
  {
    "id": 12345,
    "value": 320,
    "unit": "W",
    "date": "2025-06-15",
    "cat": "ppr",
    "record_type": "time",
    "inner_val": 300,
    "training_id": 5678,
    "training_name": "Cycling - Col du Galibier",
    "sport_id": 9,
    "sport_name": "Cyclisme"
  },
  {
    "id": 12346,
    "value": 280,
    "unit": "W",
    "date": "2025-09-01",
    "cat": "ppr",
    "record_type": "time",
    "inner_val": 1200,
    "training_id": 5890,
    "training_name": "Long ride",
    "sport_id": 9,
    "sport_name": "Cyclisme"
  }
]
```

## Errors

- `400` - Missing or invalid `cat` parameter
- `400` - Missing or invalid `record_type` (must be `time` or `distance`)
- `400` - Invalid date format for `from` or `to`
- `400` - Invalid `sports` or `items` format (must be comma-separated integers)
- `400` - Invalid `athlete_id` or not authorized to access this athlete
- `400` - Not authorized (missing `can_export_training` permission)
- `429` - Rate limit exceeded (20 requests/minute, plus standard hourly and daily limits)
