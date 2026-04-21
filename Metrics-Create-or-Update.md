Create or update a Nolio user metric

* If no metric exists for a given date, a new metric will be created

* If a metric exists for a given date, the metric will be updated

#### Endpoint:
  - **/update/metric/**
  - HTTP Method: **POST**

Example:
```
Production:
https://www.nolio.io/api/update/metric/
```
Parameters :

* metric_id: integer, required. The id for the corresponding Nolio [metric](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Metric-Object)
* new_value: integer, required.
* date_start: string, required. date of the metric. YYYY-MM-DD format
* athlete_id: integer, optional. If provided, the metric will be created/updated for the specified athlete (you must be their coach). You can retrieve athlete ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

Example:
```json
  {
    "metric_id": 1,
    "new_value": 3600,
    "date_start": "2020-08-31",
    "athlete_id": 42
  }
```

### Response
A JSON containing the metrics data that was created or updated

### Specific status code

* 400 Bad request

  * metric_id doesn’t correspond to any metric





