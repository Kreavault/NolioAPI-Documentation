Delete planned competition
#### Endpoint:
  - **/delete/planned/competition/**
  - HTTP Method: **POST**

Example:
```
Production:
https://www.nolio.io/api/delete/planned/competition/
```
Parameters :

* id_partner: integer, required.
* athlete_id: integer, optional. If provided, the planned competition will be deleted for the specified athlete (you must be their coach). You can retrieve athlete ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

Example:
```json
  {
    "id_partner": 1,
    "athlete_id": 42
  }
```

### Response
Http 200 on success

### Specific status code

* 400 Bad request