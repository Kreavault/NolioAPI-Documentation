Update training

#### Endpoint:
  - **/update/training/**
  - HTTP Method: **POST**

Example:
```
Production:
https://www.nolio.io/api/update/training/
```
Parameters :

* id_partner: integer, required.
* sport_id: integer, required. The id of the [Nolio sport](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#sport-map)
* name: string, optional.
* date_start: string, optional. date of the metric. YYYY-MM-DD format
* description: string, optional.
* duration: integer, optional. Duration of training in seconds.
* feeling : integer, optional. [Nolio feeling](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#feeling-map)
* rpe : integer, optional. [Nolio rpe](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#rpe-map)
* distance : integer, optional. distance in km
* elevation_gain : integer, optional. elevation gain in m
* athlete_id: integer, optional. If provided, the training will be updated for the specified athlete (you must be their coach). You can retrieve athlete ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

Example:
```json
  {
    "id_partner": 1,
    "sport_id": 1,
    "name": "my manual training",
    "date_start": "2020-08-01 14:54:33",
    "description": "A recovery workout"
    "duration": 36000,
    "feeling": 2,
    "rpe": 5,
    "distance": 50,
    "elevation_gain": 150
  }
```

### Response
A JSON containing the training data that was updated

### Specific status code

* 400 Bad request

  * Training doesn’t exist

