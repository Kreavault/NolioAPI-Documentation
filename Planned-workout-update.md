Update planned training
#### Endpoint:
  - **/update/planned/training/**
  - HTTP Method: **POST**

Example:
```
Production:
https://www.nolio.io/api/update/planned/training/
```
Parameters :

* id_partner: integer, required.
* sport_id: integer, required. The id of the [Nolio sport](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#sport-map)
* name: string, required.
* date_start: string, required. date of the workout. YYYY-MM-DD format
* description: string, optional. additional comment to understand what to do during this workout
* duration: integer, optional. Duration of training in seconds.
* rpe : integer, optional. [Nolio rpe](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#rpe-map)
* distance : integer, optional. distance in km
* elevation_gain : integer, optional. elevation gain in m
* structured_workout : Array, optional. see [Structured workout](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Structured-Workout)
* athlete_id: integer, optional. If provided, the planned training will be updated for the specified athlete (you must be their coach). You can retrieve athlete ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

Example:
```json
  {
    "id_partner": 1,
    "sport_id": 23,
    "name": "my planned training",
    "date_start": "2022-08-01",
    "duration": 36000,
    "rpe": 5,
    "distance": 50,
    "elevation_gain": 150
  }
```

### Response
A JSON containing the training data that was updated

### Specific status code

* 400 Bad request