# Nolio API - Full Documentation

## API Routes

## User
* [**GET** /get/user/](User-Get-Profile)
* [**GET** /get/user/meta/](User-Get-Metadata)
* [**GET** /get/athletes/](User-Get-Athletes)

## File
* [**POST** /upload/file/](File-Upload)

## Workouts
* [**GET** /get/training/](Retrieve-Workouts)
* [**GET** /get/planned/training/](Retrieve-Planned-Workouts)
* [**GET** /get/training/streams/](Retrieve-Streams-Workout)
* [**POST** /create/training/](Workout-Create)
* [**POST** /update/training/](Workout-Update)
* [**POST** /delete/training/](Workout-Delete)
* [**POST** /create/planned/training/](Planned-workout-create)
* [**POST** /update/planned/training/](Planned-workout-update)
* [**POST** /delete/planned/training/](Planned-workout-delete)
* [**POST** /create/competition/](Competition-Create)
* [**POST** /update/competition/](Competition-Update)
* [**POST** /delete/competition/](Competition-Delete)
* [**POST** /create/planned/competition/](Competition-workout-create)
* [**POST** /update/planned/competition/](Competition-workout-update)
* [**POST** /delete/planned/competition/](Competition-workout-delete)

## Metrics
* [**GET** /get/metric/](Get-Metric)
* [**POST** /update/metric/](Metrics-Create-or-Update)

## Records
* [**GET** /get/records/](Get-Records)

## Notes
* [**GET** /get/note/](Retrieve-Note)

## Objects
* [**Metric**](Metric-Object)
* [**Training**](Training-Object)
* [**Structured Workout**](Structured-Workout)

---

## Competition

### Create competition
#### Endpoint:
  - **/create/competition/**
  - HTTP Method: **POST**

Example:
```
Production:
https://www.nolio.io/api/create/competition/
```

Same arguments than [create training](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Workout-create)

### Delete competition
#### Endpoint:
  - **/delete/competition/**
  - HTTP Method: **POST**

Example:
```
Production:
https://www.nolio.io/api/delete/competition/
```
Parameters :

* id_partner: integer, required.
* athlete_id: integer, optional.

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

### Update competition
#### Endpoint:
  - **/update/competition/**
  - HTTP Method: **POST**

Same arguments than workout update

### Create planned competition
#### Endpoint:
  - **/create/planned/competition/**
  - HTTP Method: **POST**

Same arguments than create planned training

### Delete planned competition
#### Endpoint:
  - **/delete/planned/competition/**
  - HTTP Method: **POST**

Parameters :
* id_partner: integer, required.
* athlete_id: integer, optional.

Example:
```json
  {
    "id_partner": 1,
    "athlete_id": 42
  }
```

### Response
Http 200 on success

### Update planned competition
#### Endpoint:
  - **/update/planned/competition/**
  - HTTP Method: **POST**

Same arguments than planned workout update

---

## Contact

### Request Access to API
Please fill out [the access request form](https://www.nolio.io/api/register/).

### Request Support
If you need support, contact us at contact [at] nolio.io or with the [contact form](https://www.nolio.io/contact/)

---

## FAQ

### How to request access to API?
> Please use [this form](https://www.nolio.io/api/register/) to contact us.

### How to contact Nolio for support?
> Please use [this form](https://www.nolio.io/contact/) to contact us.

---

## FFC API Documentation

### Authentification

Toutes les requetes necessitent un token OAuth2 valide dans le header :

```
Authorization: Bearer <access_token>
```

Le token doit appartenir a une application OAuth2 dont le proprietaire est un administrateur FFC (DTN/SAURON).

---

### 1. Lister les sportifs FFC

#### `GET /ffc/api/athletes/`

Retourne la liste paginee de tous les sportifs FFC synchronises avec Nolio.

#### Parametres

| Parametre | Type | Defaut | Description |
|-----------|------|--------|-------------|
| `limit` | int | 500 | Nombre de resultats par page (max 2000) |
| `offset` | int | 0 | Nombre de resultats a ignorer |

#### Exemple de requete

```bash
curl -H "Authorization: Bearer <token>" \
  "https://www.nolio.io/ffc/api/athletes/?limit=100&offset=0"
```

#### Reponse

```json
{
  "count": 4523,
  "results": [
    {
      "pk": 12401,
      "nip": 45678901,
      "email": "jean.dupont@mail.com",
      "first_name": "Jean",
      "last_name": "Dupont",
      "discipline": "route",
      "category": "elite"
    }
  ]
}
```

#### Champs de la reponse

| Champ | Type | Description |
|-------|------|-------------|
| `count` | int | Nombre total de sportifs FFC |
| `results` | array | Liste des sportifs |
| `results[].pk` | int | ID Nolio du sportif (utiliser comme `athlete_id`) |
| `results[].nip` | int | Numero de licence FFC |
| `results[].email` | string | Email du compte Nolio |
| `results[].discipline` | string|null | `route`, `piste`, `vtt`, `bmx`, `cyclocross`... |
| `results[].category` | string|null | `elite`, `junior`, `espoir`... |

#### Pagination

```bash
GET /ffc/api/athletes/?limit=500&offset=0   # Page 1
GET /ffc/api/athletes/?limit=500&offset=500  # Page 2
# Continuer tant que offset < count
```

---

### 2. Recuperer les records d'un sportif FFC

#### `GET /api/get/records/`

Retourne les meilleurs records d'un sportif. L'admin FFC peut acceder aux records de n'importe quel sportif FFC via `athlete_id`.

#### Parametres

| Parametre | Type | Obligatoire | Description |
|-----------|------|-------------|-------------|
| `athlete_id` | int | oui | ID Nolio du sportif |
| `cat` | string | oui | `ppr` (puissance), `phrr` (FC), `par` (allure), `ptr` (couple), `pvar` (VAM), `pcadr` (cadence) |
| `record_type` | string | oui | `time` ou `distance` |
| `from` | string | non | Date debut `YYYY-MM-DD` |
| `to` | string | non | Date fin `YYYY-MM-DD` |
| `sports` | string | non | IDs sports separes par virgules (max 50) |
| `items` | string | non | Durees/distances specifiques (max 50) |

---

## Webhook mechanism

A webhook mechanism is available to avoid querying the API to see if there's new data.

You can setup the URLs where to receive the events in the Nolio API admin.

There are currently 2 webhooks:
* One for events in the achieved Calendar (Training, Note, Competition)
* One for new metrics

Example event for events:
```json
{
  "notif_type": "new_event",
  "object_type": "Training",
  "object_id": 32263,
  "user_id": 10,
  "date_object": "2024-12-02"
}
```

Example event for metrics:
```json
{
  "notif_type": "updated_metric",
  "object_type": "Metrics",
  "object_id": 10499,
  "user_id": 100,
  "date_object": "2024-12-03",
  "metric_type": "nombredepas"
}
```

Notif types:
* new_event
* updated_event
* deleted_event
* new_metric
* updated_metric
* deleted_metric

The webhook code is set as HTTP Header `http-x-nolio-key` to verify calls are coming from Nolio.

---

## Workout CRUD

### Create training
#### Endpoint: POST /create/training/
```
https://www.nolio.io/api/create/training/
```
Parameters:
* id_partner: integer, required
* sport_id: integer, required
* name: string, required
* date_start: string, required. YYYY-MM-DD
* description: string, optional
* duration: integer, optional. seconds
* feeling: integer, optional
* rpe: integer, optional
* distance: integer, optional. km
* elevation_gain: integer, optional. meters
* athlete_id: integer, optional

Example:
```json
{
  "id_partner": 1,
  "sport_id": 1,
  "name": "my manual training",
  "date_start": "2020-08-01",
  "duration": 36000,
  "feeling": 2,
  "rpe": 5,
  "distance": 50,
  "elevation_gain": 150
}
```
Response: JSON with training data created

### Delete training
#### Endpoint: POST /delete/training/
Parameters:
* id_partner: integer, required
* athlete_id: integer, optional

### Update training
#### Endpoint: POST /update/training/
Parameters: same as create, all optional except id_partner and sport_id

---

## User Profile

### GET /get/user/
Parameters:
* athlete_id: integer, optional

Example:
```
https://www.nolio.io/api/get/user/
https://www.nolio.io/api/get/user/?athlete_id=42
```

Response:
```json
{
  "id": 42,
  "first_name": "Vincent",
  "last_name": "Luis",
  "birthday": "1990-02-12"
}
```
