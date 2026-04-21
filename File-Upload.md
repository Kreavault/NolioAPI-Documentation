Upload .fit or .tcx file

#### Endpoint:
  - **/upload/file/**
  - HTTP Method: **POST**

Example:
```
Production:
https://www.nolio.io/api/upload/file/
```
Parameters :

* id_partner: string, required. A unique id for the training across all your application
* format: string, required. Must be `fit` or `tcx`
* data: string, required.
  - **Base64** encoded file contents.
* title: string, optional. 
  - Optional title that will be used for the training.
* comment: string, optional.
  - Optional comment that will be added to the training.
* athlete_id: integer, optional. If provided, the file will be imported for the specified athlete (you must be their coach). You can retrieve athlete ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

Example:
```json
{
    "id_partner" : 345700,
    "data": "BleKx...",
    "title": "My workout",
    "Comment": "My workout desc"
}
```

### Specific status code

* `202 Accepted`
  * Returned if the request is accepted, files upload are sent in a queue to be processed

* 400 Bad request

  * Training already imported

  * .tcx files not authorized, you need to ask permission to contact@nolio.io





