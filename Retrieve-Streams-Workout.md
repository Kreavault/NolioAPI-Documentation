**You need a special authorization to use this endpoint, ask it to contact@nolio.io**

#### Endpoint:
  - **/get/training/streams/**
  - HTTP Method: **GET**

Parameters :

    id: integer, required (nolio_id workout)
    athlete_id: integer, optional, default = current user. Can be used to retrieve data from one of your managed athletes. You can retrieve the ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

nolio_id can be retrieve through [/get/training/](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Retrieve-Workout) endpoint

Example:
```
Production:
https://www.nolio.io/api/get/training/streams/?id=4998195
```

Returns: Workout streams data
```json
{
  "file_url": "<file_url>",
  "stream_heartrate": [
    149, 152, 153, 150, 150, 153, 153, 151, 152
  ],
  "stream_torque": [
    ...
  ],
  "stream_watts": [
    ...
  ],
  "stream_cadence": [
    ...
  ],
  "stream_pace": [
    ...
  ],
  "stream_altitude": [
    ...
  ],
  "stream_distance": [
    ...
  ],
  "stream_time": [
    ...
  ]
}
```