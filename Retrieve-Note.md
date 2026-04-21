**You need a special authorization to use this endpoint, ask it to contact@nolio.io**

#### Endpoint:
  - **/get/note/**
  - HTTP Method: **GET**

Parameters :

    note_type: string, optional, filter specific note type, see below
    id: int, optional. if you already have a note id you can put here to get only that note
    limit: integer, optional, default = 30 : maximum number of workouts returned
    from: string, optional, date format: "YYYY-MM-DD" : get all workouts after this date
    to: string, optional, date format: "YYYY-MM-DD", can be combined with from : get all workouts before this date
    athlete_id: integer, optional, default = current user. can be used to retrieve data from one of your managed athlete, you can retrieve the ids with the Get_athletes route -> https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes


Returned notes are ordered from the last one


Example:
```
Production:
https://www.nolio.io/api/get/note/?from=2021-04-03&to=2021-04-04&limit=2&note_type=blessure
```
## Possible note type

    'blessure' 
    'dispo'
    'inconfort'
    'goal'
    'indispo'
    'malade'
    'menstruel'
    'repos'


Returns: An athlete’s note
```json
[{
"nolio_id": 819948, 
"name": "Covid19", 
"type": "Malade", 
"date_start": "2024-10-28", 
"hour_start": "", 
"description": "", 
"sick_type": "Covid19"
}]
```