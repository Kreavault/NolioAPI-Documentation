#### Endpoint:
  - **/get/athletes/**
  - HTTP Method: **GET**

Parameters :

* wants_coach: bool : true if you want to include coach


Example:
```
Production:
https://www.nolio.io/api/get/athletes/?wants_coach=false
```

Returns: Athlete’s profile
```json
[{
 "nolio_id": 1,
 "name": "Vincent Luis"
},
{
 "nolio_id": 2,
 "name": "Ugo Ferrari"
}]

```