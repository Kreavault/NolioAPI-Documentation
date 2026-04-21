**You need a special authorization to use this endpoint, ask it to contact@nolio.io**

#### Endpoint:
  - **/get/training/**
  - HTTP Method: **GET**

Parameters :

    limit: integer, optional, default = 30 : maximum number of workouts returned
    id: int, optional. if you already have a training id you can put here to get only that training
    from: string, optional, date format: "YYYY-MM-DD" : get all workouts after this date
    to: string, optional, date format: "YYYY-MM-DD", can be combined with from : get all workouts before this date
    athlete_id: integer, optional, default = current user. can be used to retrieve data from one of your managed athlete, you can retrieve the ids with the Get_athletes route -> https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes


Returned workouts are ordered from the last one


Example:
```
Production:
https://www.nolio.io/api/get/training/?from=2021-04-03&to=2021-04-04&limit=2
```

See [Training](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object)

Returns: An athlete’s workouts
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
    "load_foster": 342.4166666666667,
    "load_coggan": 68.48333333333332,
    "rest_hr_user": 50,
    "rest_max_user": 190,
    "np": 0,
    "ftp": 305.0,
    "rftp": 0,
    "weight": 66.0,
    "critical_power": 303.0,
    "wbal": 22000.0,
    "file_url": "<file_url>",
    "planned_name": "CaP - Fartleck Trail",
    "planned_sport": "Running",
    "planned_sport_id": 2,
    "planned_description": "",
    "planned_load_coggan": 0
  },
  {
    "nolio_id": 4986055,
    "name": "Bike",
    "sport": "Bike",
    "sport_id": 14,
    "date_start": "2021-04-03",
    "duration": 12965,
    "distance": 77.71,
    "rpe": 3,
    "feeling": 4,
    "description": "",
    "load_foster": 648.25,
    "load_coggan": 131.65223201172338,
    "rest_hr_user": 50,
    "rest_max_user": 180,
    "np": 184.40765380859375,
    "ftp": 305.0,
    "rftp": 0,
    "weight": 66.0,
    "critical_power": 303.0,
    "wbal": 22000.0,
    "file_url": "<file_url>",
    "planned_name": "Bike",
    "planned_sport": "Bike,
    "planned_sport_id": 14,
    "planned_description": "",
    "planned_load_coggan": 0
  }
]
```