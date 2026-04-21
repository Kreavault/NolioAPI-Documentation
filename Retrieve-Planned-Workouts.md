**You need a special authorization to use this endpoint, ask it to contact@nolio.io**

#### Endpoint:
  - **/get/planned/training/**
  - HTTP Method: **GET**

Parameters :

    limit: integer, optional, default = 30 : maximum number of workouts returned
    id: int, optional. if you already have a planned_training id you can put here to get only that planned training
    from: string, optional, date format: "YYYY-MM-DD" : get all workouts after this date
    to: string, optional, date format: "YYYY-MM-DD", can be combined with from : get all workouts before this date
    athlete_id: integer, optional, default = current user. Can be used to retrieve data from one of your managed athletes. You can retrieve the ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.


Returned workouts are ordered from the last one


Example:
```
Production:
https://www.nolio.io/api/get/planned/training/?from=2021-04-03&to=2021-04-04&limit=2
```

See [Training](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object)

Returns: An athlete’s planned workouts
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
    "sport": "Course à pied",
    "sport_id": 2,
    "date_start": "2023-03-23",
    "hour_start": "",
    "duration": 0,
    "distance": 12.0,
    "rpe": 2,
    "description": "",
    "elevation_gain": 0,
    "elevation_loss": 0,
    "load_foster": 0,
    "load_coggan": 27.0,
    "is_competition": false,
    "structured_workout": [
      {
        "step_duration_type": "distance",
        "step_duration_value": 3000,
        "target_value_max": 3.45303867410221,
        "target_value_min": 2.5322283610082876,
        "intensity_type": "warmup",
        "target_type": "pace"
      },
      {
        "type": "repetition",
        "value": 3,
        "steps": [
          {
            "step_duration_type": "distance",
            "open_duration": true,
            "step_duration_value": 4000,
            "target_value_max": 4.235727440232044,
            "target_value_min": 4.05156537761326,
            "intensity_type": "active",
            "target_type": "pace"
          },
          {
            "step_duration_type": "distance",
            "step_duration_value": 500,
            "target_value_max": 3.6832412523756908,
            "target_value_min": 2.992633517555249,
            "target_type": "pace"
          }
        ]
      }
    ]
  },
  {
    "nolio_id": 6688554,
    "name": "tempo-Z2 (recyclage des lactates)",
    "sport": "Course à pied",
    "sport_id": 2,
    "date_start": "2023-03-21",
    "hour_start": "",
    "duration": 4800,
    "distance": 17.389,
    "rpe": 5,
    "description": "But de la séance :\\xa0Cette séance alterne 2 intensités différentes pour accumuler du lactate afin de renforcer la progression et repousser le seuil de fatigue.L'alternance de stimulation à pour but d'améliorer le recyclage du lactate.",
    "elevation_gain": 0,
    "elevation_loss": 0,
    "load_foster": 400.0,
    "load_coggan": 80.0,
    "is_competition": false,
    "structured_workout": [
      {
        "step_duration_type": "duration",
        "step_duration_value": 900,
        "target_value_max": 3.45303867410221,
        "target_value_min": 2.5322283610082876,
        "target_type": "pace"
      },
      {
        "type": "repetition",
        "value": 4,
        "steps": [
          {
            "type": "repetition",
            "value": 4,
            "steps": [
              {
                "step_duration_type": "duration",
                "step_duration_value": 30,
                "target_value_max": 5.064456722016575,
                "target_value_min": 4.6040515654696135,
                "intensity_type": "active",
                "target_type": "pace"
              },
              {
                "step_duration_type": "duration",
                "step_duration_value": 120,
                "target_value_max": 4.143646408922653,
                "target_value_min": 3.9134438306491712,
                "intensity_type": "active",
                "target_type": "pace"
              }
            ]
          },
          {
            "step_duration_type": "duration",
            "step_duration_value": 300,
            "target_value_max": 3.45303867410221,
            "target_value_min": 2.762430939281768,
            "target_type": "pace"
          }
        ]
      },
      {
        "step_duration_type": "duration",
        "step_duration_value": 300,
        "target_value_max": 3.45303867410221,
        "target_value_min": 2.5322283610082876,
        "target_type": "pace"
      }
    ]
  }
]
```