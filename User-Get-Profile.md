#### Endpoint:
  - **/get/user/**
  - HTTP Method: **GET**

Parameters :

    athlete_id: integer, optional, default = current user. Can be used to retrieve data from one of your managed athletes. You can retrieve the ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

Example:
```
Production:
https://www.nolio.io/api/get/user/
https://www.nolio.io/api/get/user/?athlete_id=42
```

Returns: An athlete’s profile
```json
{
 "id": 42,
 "first_name": "Vincent",
 "last_name": "Luis",
 "birthday": "1990-02-12"
}

```