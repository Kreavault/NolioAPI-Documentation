**You need a special authorization to use this endpoint, ask it to contact@nolio.io**

#### Endpoint:
  - **/get/metric/**
  - HTTP Method: **GET**

Parameters :
    id: int, mandatory. the metric id
    athlete_id: integer, optional, default = current user. Can be used to retrieve data from one of your managed athletes. You can retrieve the ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

Example:
```
Production:
https://www.nolio.io/api/get/metric/?id=34
```

See [Metric](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Metric-Object)

Returns: The metric 
```json
{"date": "2020-05-15", "hour": "", "value": 64.3, "unit": "kg", "type": "Poids"}
```