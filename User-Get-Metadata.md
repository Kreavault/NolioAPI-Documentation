**You need a special authorization to use this endpoint, ask it to contact@nolio.io**

#### Endpoint:
  - **/get/user/meta/**
  - HTTP Method: **GET**

Parameters :

* limit: integer, optional, default = 15 : maximum number of values returned for each metrics type
* from: string, optional, date format: "YYYY-MM-DD" : get all metrics after this date
* to: string, optional, date format: "YYYY-MM-DD", can be combined with from : get all metrics before this date
* athlete_id: integer, optional, default = current user. Can be used to retrieve data from one of your managed athletes. You can retrieve the ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.


Example:
```
Production:
https://www.nolio.io/api/get/user/meta/?limit=20
```

Returns: An athlete’s metadata (goals & metrics)

See [Training](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object)

**Date** are in **YYYT-MM-JJ** format

**Hours** are in **HH:MM** format with H in [0..24] & M in [0..59]

```json
{
   "goals":[
      
   ],
   "weight":{
      "unit":"kg",
      "data":[
         {
            "date":"2021-02-05",
            "hour":"13:30",
            "value":55.7
         },
         {
            "date":"2020-07-28",
            "hour":null,
            "value":56.5
         },
         {
            "date":"2020-05-03",
            "hour":null,
            "value":57.0
         }
      ]
   },
   "hrmax":{
      "unit":"bpm",
      "data":[
         {
            "date":"2019-02-21",
            "hour":null,
            "value":176.0
         }
      ]
   },
   "sleep":{
      "unit":"seconds",
      "data":[
         {
            "date":"2020-04-07",
            "hour":"10:00",
            "value":30600.0
         },
         {
            "date":"2020-04-07",
            "hour":"14:25",
            "value":4200.0
         },
         {
            "date":"2020-03-23",
            "hour":null,
            "value":32400.0
         }
      ]
   },
   "hrrest":{
      "unit":"bpm",
      "data":[
         {
            "date":"2019-01-14",
            "hour":null,
            "value":48.0
         }
      ]
   },
   "aerobicspeed":{
      "unit":"km/h",
      "data":[
         {
            "date":"2021-04-15",
            "hour":null,
            "value":16.5
         },
         {
            "date":"2020-07-16",
            "hour":null,
            "value":15.5
         }
      ]
   },
   "ftp":{
      "unit":"W",
      "data":[
         {
            "date":"2020-12-09",
            "hour":null,
            "value":182.0
         }
      ]
   },
   "criticalpowercycling":{
      "unit":"W",
      "data":[
         {
            "date":"2021-01-07",
            "hour":null,
            "value":183.0
         },
         {
            "date":"2020-12-09",
            "hour":null,
            "value":176.0
         }
      ]
   },
   "wbalcycling":{
      "unit":"J",
      "data":[
         {
            "date":"2021-01-07",
            "hour":null,
            "value":10800.0
         },
         {
            "date":"2020-12-09",
            "hour":null,
            "value":19200.0
         }
      ]
   }
}
```