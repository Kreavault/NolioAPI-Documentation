# Nolio API - Full Documentation
# Source: NolioApp/NolioAPI-Documentation wiki (all 35 pages)

---

## API Routes

### User
- GET /get/user/
- GET /get/user/meta/
- GET /get/athletes/

### File
- POST /upload/file/

### Workouts
- GET /get/training/
- GET /get/planned/training/
- GET /get/training/streams/
- POST /create/training/
- POST /update/training/
- POST /delete/training/
- POST /create/planned/training/
- POST /update/planned/training/
- POST /delete/planned/training/
- POST /create/competition/
- POST /update/competition/
- POST /delete/competition/
- POST /create/planned/competition/
- POST /update/planned/competition/
- POST /delete/planned/competition/

### Metrics
- GET /get/metric/
- POST /update/metric/

### Records
- GET /get/records/

### Notes
- GET /get/note/

---

## OAuth 2.0

Nolio API uses OAuth 2.0. Each app gets client_id and client_secret.

### OAuth URLs
- Authorize: https://www.nolio.io/api/authorize/
- Token Exchange: https://www.nolio.io/api/token/
- Deauthorize: https://www.nolio.io/api/deauthorize/

### Auth flow
1. GET https://www.nolio.io/api/authorize/?response_type=code&client_id=XXX&redirect_uri=YYY
2. User approves
3. Redirect to redirect_uri?code=ABC123
4. POST /api/token/ with Authorization: Basic base64(client_id:client_secret) and body: grant_type=authorization_code&code=ABC123&redirect_uri=YYY

### Token response
```json
{
  "access_token": "gAAAAMYien...",
  "token_type": "bearer",
  "expires_in": 86400,
  "refresh_token": "i7ne!IAAA...",
  "scope": "read write"
}
```

### Refresh token
POST /api/token/ with grant_type=refresh_token&refresh_token=XXX + same Basic auth header

### All requests
```
Authorization: Bearer <access_token>
Content-Type: application/json
Accept: application/json
```

---

## Rate Limits

### Dev app
- 200 req/hour, 2000 req/day

### Production app
- 500 + 20*nb_users req/hour
- 5000 + 100*nb_users req/day

### HTTP Status codes
- 200: OK
- 400: Bad request
- 401: Unauthorized (bad/expired token)
- 403: Forbidden
- 429: Too many requests
- 500: Server error
- 503: Service unavailable

---

## GET /get/training/
**Requires special authorization**

Parameters:
- limit: int, optional, default=30
- id: int, optional
- from: string, YYYY-MM-DD, optional
- to: string, YYYY-MM-DD, optional
- athlete_id: int, optional

Returns array of workouts:
```json
[
  {
    "nolio_id": 4998195,
    "name": "CaP - Fartleck Trail",
    "sport": "Running",
    "sport_id": 2,
    "date_start": "2021-04-04",
    "duration": 4109,
    "distance": 11.97,
    "rpe": 5,
    "feeling": 4,
    "description": "The place to be",
    "load_foster": 342.41,
    "load_coggan": 68.48,
    "rest_hr_user": 50,
    "rest_max_user": 190,
    "np": 0,
    "ftp": 305.0,
    "rftp": 0,
    "weight": 66.0,
    "critical_power": 303.0,
    "wbal": 22000.0,
    "file_url": "<url valid 1h>",
    "planned_name": "CaP - Fartleck Trail",
    "planned_sport": "Running",
    "planned_sport_id": 2,
    "planned_description": "",
    "planned_load_coggan": 0,
    "is_competition": false
  }
]
```

---

## GET /get/planned/training/
**Requires special authorization**

Parameters:
- limit: int, optional, default=30
- id: int, optional
- from: string, YYYY-MM-DD, optional
- to: string, YYYY-MM-DD, optional
- athlete_id: int, optional

Returns array of planned workouts:
```json
[
  {
    "nolio_id": 6688558,
    "name": "50km bretagne",
    "sport": "Trail",
    "sport_id": 52,
    "date_start": "2023-03-25",
    "hour_start": "",
    "duration": 0,
    "distance": 0,
    "rpe": 0,
    "description": "",
    "elevation_gain": 0,
    "elevation_loss": 0,
    "load_foster": 0,
    "load_coggan": 0,
    "is_competition": true
  },
  {
    "nolio_id": 6688556,
    "name": "LIT 12km",
    "sport": "Course a pied",
    "sport_id": 2,
    "date_start": "2023-03-23",
    "duration": 0,
    "distance": 12.0,
    "rpe": 2,
    "load_coggan": 27.0,
    "is_competition": false,
    "structured_workout": [...]
  }
]
```

---

## GET /get/training/streams/
**Requires special authorization**

Parameters:
- id: int, required (nolio_id)
- athlete_id: int, optional

Returns:
```json
{
  "file_url": "<url>",
  "stream_heartrate": [149, 152, ...],
  "stream_torque": [...],
  "stream_watts": [...],
  "stream_cadence": [...],
  "stream_pace": [...],
  "stream_altitude": [...],
  "stream_distance": [...],
  "stream_time": [...]
}
```

---

## POST /create/training/

Parameters:
- id_partner: int, required
- sport_id: int, required
- name: string, required
- date_start: string, required (YYYY-MM-DD)
- description: string, optional
- duration: int, optional (seconds)
- feeling: int, optional (1-5)
- rpe: int, optional (1-10)
- distance: int, optional (km)
- elevation_gain: int, optional (m)
- athlete_id: int, optional

```json
{
  "id_partner": 1,
  "sport_id": 14,
  "name": "Road ride",
  "date_start": "2024-01-15",
  "duration": 7200,
  "feeling": 4,
  "rpe": 6,
  "distance": 80
}
```

---

## POST /update/training/

Same params as create, id_partner and sport_id required, rest optional.

---

## POST /delete/training/

Parameters:
- id_partner: int, required
- athlete_id: int, optional

---

## POST /create/planned/training/

Parameters:
- id_partner: int, required
- sport_id: int, required
- name: string, required
- date_start: string, required (YYYY-MM-DD)
- description: string, optional
- duration: int, optional (seconds)
- rpe: int, optional (1-10)
- distance: int, optional (km)
- elevation_gain: int, optional (m)
- structured_workout: array, optional
- athlete_id: int, optional

---

## POST /update/planned/training/

Same params as create planned.

---

## POST /delete/planned/training/

Parameters:
- id_partner: int, required
- athlete_id: int, optional

---

## POST /upload/file/

Parameters:
- id_partner: string, required (unique id)
- format: string, required ("fit" or "tcx")
- data: string, required (base64 encoded)
- title: string, optional
- comment: string, optional
- athlete_id: int, optional

Returns: 202 Accepted (async processing)

---

## GET /get/user/

Parameters:
- athlete_id: int, optional

Returns:
```json
{
  "id": 42,
  "first_name": "Vincent",
  "last_name": "Luis",
  "birthday": "1990-02-12"
}
```

---

## GET /get/user/meta/
**Requires special authorization**

Parameters:
- limit: int, optional, default=15
- from: string, YYYY-MM-DD, optional
- to: string, YYYY-MM-DD, optional
- athlete_id: int, optional

Returns athlete metadata (goals & metrics) including: weight, hrmax, sleep, hrrest, aerobicspeed, ftp, criticalpowercycling, wbalcycling, etc.

---

## GET /get/athletes/

Parameters:
- wants_coach: bool

Returns:
```json
[
  {"nolio_id": 1, "name": "Vincent Luis"},
  {"nolio_id": 2, "name": "Ugo Ferrari"}
]
```

---

## GET /get/metric/
**Requires special authorization**

Parameters:
- id: int, required
- athlete_id: int, optional

Returns: `{"date": "2020-05-15", "hour": "", "value": 64.3, "unit": "kg", "type": "Poids"}`

---

## POST /update/metric/

Parameters:
- metric_id: int, required
- new_value: int, required
- date_start: string, required (YYYY-MM-DD)
- athlete_id: int, optional

---

## GET /get/records/
**Requires special authorization — rate limit: 20 req/min**

Parameters:
- cat: string, required. One of: ppr (power), phrr (heart rate), par (pace), ptr (torque), pvar (vertical ascent speed), pcadr (cadence)
- record_type: string, required. "time" or "distance"
- from: string, optional (YYYY-MM-DD)
- to: string, optional (YYYY-MM-DD)
- sports: string, optional (comma-separated sport IDs)
- items: string, optional (comma-separated durations in seconds or distances in meters)
- athlete_id: int, optional

### Time items (seconds): 1,2,3,4,5,6,7,8,9,10,15,20,25,30,45,60,120,180,240,300,360,420,480,540,600,720,900,1200,1500,1800,2700,3600,5400,7200,9000,10800,12600,14400,16200,18000...

### Distance items (meters): 400,500,800,1000,1500,1609,2000,3000,3218,5000,8045,10000,15000,16090,20000,21097,25000,30000,35000,40000,42195,50000,60000,80450,100000,160900

Returns array of record objects:
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
    "sport_id": 14,
    "sport_name": "Road cycling"
  }
]
```

---

## GET /get/note/
**Requires special authorization**

Parameters:
- note_type: string, optional (blessure, dispo, inconfort, goal, indispo, malade, menstruel, repos)
- id: int, optional
- limit: int, optional, default=30
- from: string, YYYY-MM-DD, optional
- to: string, YYYY-MM-DD, optional
- athlete_id: int, optional

Returns:
```json
[{
  "nolio_id": 819948,
  "name": "Covid19",
  "type": "Malade",
  "date_start": "2024-10-28",
  "hour_start": "",
  "description": "",
  "sick_type": "Covid19"
}]
```

---

## Webhook mechanism

2 webhooks available (configure URLs in Nolio API admin):
1. Calendar events (Training, Note, Competition)
2. New metrics

Security: `http-x-nolio-key` header to verify origin.

### Event payload
```json
{
  "notif_type": "new_event",
  "object_type": "Training",
  "object_id": 32263,
  "user_id": 10,
  "date_object": "2024-12-02"
}
```

### Metric payload
```json
{
  "notif_type": "updated_metric",
  "object_type": "Metrics",
  "object_id": 10499,
  "user_id": 100,
  "date_object": "2024-12-03",
  "metric_type": "nombredepas"
}
```

notif_type values: new_event, updated_event, deleted_event, new_metric, updated_metric, deleted_metric

---

## Sport map

| Sport | ID |
|---|---|
| Running | 2 |
| XC ski - Classic | 3 |
| XC ski - Skating | 4 |
| Roller ski - Classic | 5 |
| Roller ski - Skating | 6 |
| Ski Mountaineering | 7 |
| Climbing | 8 |
| Bodybuilding | 10 |
| Other | 12 |
| Road cycling | 14 |
| Mountain cycling | 15 |
| Hiking | 16 |
| Virtual ride | 18 |
| Swimming | 19 |
| Strength | 20 |
| Stretching | 21 |
| Treadmill | 24 |
| Kayaking - Sea | 26 |
| Kayaking - River | 27 |
| Elliptical trainer | 28 |
| Walking sticks | 29 |
| Yoga | 30 |
| Canoe - Sea | 31 |
| Canoe - River | 32 |
| Rowing | 33 |
| Orienteering race | 34 |
| Track cycling | 35 |
| CX cycling | 36 |
| Squash | 37 |
| Biathlon | 38 |
| Walking | 45 |
| Stand up paddle | 51 |
| Trail running | 52 |
| OCR running | 53 |
| Tennis | 59 |

---

## Feeling map
| Value | Label |
|---|---|
| 1 | Very weak |
| 2 | Weak |
| 3 | Normal |
| 4 | Good |
| 5 | Strong |

## RPE map
| Value | Label |
|---|---|
| 1 | Very easy |
| 2 | Easy |
| 3 | Easy |
| 4 | Moderate |
| 5 | Moderate |
| 6 | Hard |
| 7 | Hard |
| 8 | Very hard |
| 9 | Very hard |
| 10 | All out |

---

## Metric Object

| Metric | ID | Unit |
|---|---|---|
| Sleep | 1 | seconds |
| Weight | 2 | kg |
| Body fat % | 3 | % |
| Max heartrate | 4 | bpm |
| VMA (Maximal Aerobic Speed) | 5 | km/h |
| PMA (Maximal Aerobic Power) | 6 | watts |
| FTP | 7 | watts |
| VO2MAX | 8 | ml/min/kg |
| Resting heartrate | 9 | bpm |
| rFTP (running FTP) | 28 | watts |

---

## Training Object - Full field list (for retrieval)

| Field | Type | Unit | Notes |
|---|---|---|---|
| nolio_id | int | | use for other endpoints |
| name | string | | |
| sport | string | | |
| sport_id | int | | |
| date_start | date | | YYYY-MM-DD |
| hour_start | time | | HH:MM:SS |
| description | string | | |
| duration | int | seconds | |
| distance | float | km | |
| rpe | int | 1-10 | |
| feeling | int | 1-5 | |
| elevation_gain | int | m | |
| load_foster | float | | |
| load_coggan | float | | |
| rest_hr_user | int | bpm | user resting HR |
| max_hr_user | int | bpm | user max HR |
| np | float | W | normalized power |
| ftp | float | W | |
| rftp | float | W | running FTP |
| weight | float | kg | |
| critical_power | float | W | |
| wbal | float | J | |
| file_url | url | | valid 1h only |
| planned_name | string | | |
| planned_sport | string | | |
| planned_sport_id | int | | |
| planned_description | string | | |
| planned_load_coggan | float | | |
| is_competition | bool | | |

---

## Structured Workout Format

A workout is an array of steps. Step types:

### 1. Atomic step
| Field | Type | Description |
|---|---|---|
| step_duration_type | "duration" or "distance" | |
| step_duration_value | number | seconds or meters |
| open_duration | boolean | optional |
| target_value_min | number | lower bound |
| target_value_max | number | upper bound (mandatory if not no_target) |
| intensity_type | string | warmup, cooldown, active, rest, ramp_up, ramp_down |
| target_type | string | pace, speed, power, heartrate, no_target |
| rpe | number | optional |
| comment | string | optional, \n for linebreak |
| secondary_step | atomic step | optional (e.g. cadence + power) |

### 2. Repetition
| Field | Type | Description |
|---|---|---|
| type | "repetition" | required |
| value | number | nb of repetitions |
| steps | array | repeated steps |

### 3. Exercise
| Field | Type |
|---|---|
| type | "exercise" |
| name | string |
| instructions | string |
| sets | array of sets |

### Units
| Target | Unit |
|---|---|
| pace | mps (meters/second) |
| speed | km/h |
| power | W |
| heartrate | bpm |
| cadence | rpm |
| step_duration_value | meter or second |

### Example
```json
{
  "structured_workout": [
    {
      "intensity_type": "warmup",
      "step_duration_type": "duration",
      "step_duration_value": 600,
      "type": "step",
      "target_type": "power",
      "target_value_min": 200,
      "target_value_max": 250,
      "open_duration": true
    },
    {
      "type": "repetition",
      "value": 3,
      "steps": [
        {
          "intensity_type": "active",
          "step_duration_type": "duration",
          "step_duration_value": 180,
          "type": "step",
          "target_type": "power",
          "target_value_min": 300,
          "target_value_max": 350
        },
        {
          "intensity_type": "rest",
          "step_duration_type": "duration",
          "step_duration_value": 180,
          "type": "step",
          "target_type": "no_target"
        }
      ]
    }
  ]
}
```

---

## FFC API (admin only)

### GET /ffc/api/athletes/
Returns paginated list of FFC athletes.

Parameters: limit (default 500, max 2000), offset

Response fields: pk (=athlete_id), nip (license), email, first_name, last_name, discipline, category

### Records access
FFC admin can access records of any FFC athlete via athlete_id without being their coach.

---

## Research Project API

Auth: Bearer secret key (not OAuth)

Endpoints:
- GET /api/research/participants/
- GET /groups/
- POST /update/groups/
- POST /create/message/
- GET /api/research/{training|competition}/{user_id}/
- GET /api/research/training/{pk}/streams/
- GET /api/research/notes/{user_id}/
- GET /api/research/planned/notes/{user_id}/
- GET /api/research/metrics/{user_id}/
- GET /api/research/planned/{training|competition}/{user_id}/
- POST /api/research/create/{training|competition}/planned/{user_id}/
- POST /api/research/update/{training|competition}/planned/{user_id}/
- POST /api/research/delete/{training|competition}/planned/{user_id}/
- POST /api/research/create/trainingplan/
- POST /api/research/update/trainingplan/{user_id}/{plan_id}/
- POST /api/research/delete/trainingplan/{user_id}/{plan_id}/
