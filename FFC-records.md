# API FFC — Documentation

## Authentification

Toutes les requêtes nécessitent un token OAuth2 valide dans le header :

```
Authorization: Bearer <access_token>
```

Le token doit appartenir à une application OAuth2 dont le propriétaire est un administrateur FFC (DTN/SAURON).

---

## 1. Lister les sportifs FFC

### `GET /ffc/api/athletes/`

Retourne la liste paginée de tous les sportifs FFC synchronisés avec Nolio.

### Paramètres

| Paramètre | Type | Défaut | Description |
|-----------|------|--------|-------------|
| `limit` | int | 500 | Nombre de résultats par page (max 2000) |
| `offset` | int | 0 | Nombre de résultats à ignorer |

### Exemple de requête

```bash
curl -H "Authorization: Bearer <token>" \
  "https://www.nolio.io/ffc/api/athletes/?limit=100&offset=0"
```

### Réponse

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
    },
    {
      "pk": 12402,
      "nip": 45678902,
      "email": "marie.martin@mail.com",
      "first_name": "Marie",
      "last_name": "Martin",
      "discipline": "piste",
      "category": "junior"
    }
  ]
}
```

### Champs de la réponse

| Champ | Type | Description |
|-------|------|-------------|
| `count` | int | Nombre total de sportifs FFC |
| `results` | array | Liste des sportifs (page courante) |
| `results[].pk` | int | ID Nolio du sportif (à utiliser comme `athlete_id` sur les autres routes) |
| `results[].nip` | int | Numéro de licence FFC |
| `results[].email` | string | Email du compte Nolio |
| `results[].first_name` | string | Prénom |
| `results[].last_name` | string | Nom |
| `results[].discipline` | string \| null | Discipline (`"route"`, `"piste"`, `"vtt"`, `"bmx"`, `"cyclocross"`, ...) |
| `results[].category` | string \| null | Catégorie (`"elite"`, `"junior"`, `"espoir"`, ...) |

### Pagination

Pour récupérer tous les sportifs, itérer avec `offset` :

```bash
# Page 1
GET /ffc/api/athletes/?limit=500&offset=0

# Page 2
GET /ffc/api/athletes/?limit=500&offset=500

# Page 3
GET /ffc/api/athletes/?limit=500&offset=1000

# Continuer tant que offset < count
```

---

## 2. Récupérer les records d'un sportif FFC

### `GET /api/get/records/`

Retourne les meilleurs records d'un sportif, filtrés par catégorie de donnée, type de record, période et sport.

L'admin FFC peut accéder aux records de **n'importe quel sportif FFC** via le paramètre `athlete_id`, sans avoir besoin d'être son coach.

### Paramètres

| Paramètre | Type | Obligatoire | Description |
|-----------|------|-------------|-------------|
| `athlete_id` | int | oui | ID Nolio du sportif (le `pk` retourné par la route athletes) |
| `cat` | string | oui | Catégorie de record : `ppr` (puissance), `phrr` (fréquence cardiaque), `par` (allure), `ptr` (couple), `pvar` (vitesse ascensionnelle), `pcadr` (cadence) |
| `record_type` | string | oui | Type de record : `time` (meilleur sur une durée) ou `distance` (meilleur sur une distance) |
| `from` | string | non | Date de début au format `YYYY-MM-DD`. Si omis, pas de borne inférieure. |
| `to` | string | non | Date de fin au format `YYYY-MM-DD`. Si omis, pas de borne supérieure. |
| `sports` | string | non | IDs de sports séparés par des virgules (ex: `1,2,3`). Max 50. |
| `items` | string | non | Durées/distances spécifiques séparées par des virgules (ex: `5,20,60` pour 5s, 20s, 60s en `time`). Max 50. |

### Exemple de requête

```bash
# Records de puissance (PPR) sur des durées, pour le sportif 12401
curl -H "Authorization: Bearer <token>" \
  "https://www.nolio.io/api/get/records/?athlete_id=12401&cat=ppr&record_type=time"

# Records de puissance de 2025 uniquement
curl -H "Authorization: Bearer <token>" \
  "https://www.nolio.io/api/get/records/?athlete_id=12401&cat=ppr&record_type=time&from=2025-01-01&to=2025-12-31"

# Records de puissance sur 5s, 1min et 5min
curl -H "Authorization: Bearer <token>" \
  "https://www.nolio.io/api/get/records/?athlete_id=12401&cat=ppr&record_type=time&items=5,60,300"
```

### Réponse

```json
[
  {
    "id": 89012,
    "value": 420,
    "unit": "W",
    "date": "2025-06-15",
    "cat": "ppr",
    "record_type": "time",
    "inner_val": 5,
    "training_id": 567890,
    "training_name": "Sortie col du Tourmalet",
    "sport_id": 1,
    "sport_name": "Cyclisme"
  },
  {
    "id": 89013,
    "value": 380,
    "unit": "W",
    "date": "2025-07-02",
    "cat": "ppr",
    "record_type": "time",
    "inner_val": 60,
    "training_id": 567920,
    "training_name": "Interval training",
    "sport_id": 1,
    "sport_name": "Cyclisme"
  }
]
```

### Champs de la réponse

| Champ | Type | Description |
|-------|------|-------------|
| `id` | int | ID du record |
| `value` | number | Valeur du record |
| `unit` | string | Unité (`"W"`, `"bpm"`, `"mps"`, `"N.m"`, `"m/h"`) |
| `date` | string \| null | Date du record (`YYYY-MM-DD`) |
| `cat` | string | Catégorie du record |
| `record_type` | string | `"time"` ou `"distance"` |
| `inner_val` | int | Durée en secondes (si `record_type=time`) ou distance en mètres (si `record_type=distance`). Présent uniquement si applicable. |
| `training_id` | int | ID de la séance associée (si applicable) |
| `training_name` | string | Nom de la séance (si applicable) |
| `sport_id` | int | ID du sport (si applicable) |
| `sport_name` | string | Nom du sport (si applicable) |

### Catégories et unités

| `cat` | Description | Unité | Valeurs typiques |
|--------|-------------|-------|------------------|
| `ppr` | Puissance | W (watts) | `value` arrondi à l'entier |
| `phrr` | Fréquence cardiaque | bpm | `value` arrondi à l'entier |
| `par` | Allure | mps (mètres/seconde) | `value` en décimal |
| `ptr` | Couple | N.m (newton-mètres) | `value` arrondi |
| `pvar` | Vitesse ascensionnelle | m/h (mètres/heure) | `value` arrondi |
| `pcadr` | Cadence | rpm ou spm (selon le sport) | `value` arrondi à l'entier |

---

## Codes d'erreur

| Code | Description |
|------|-------------|
| 400 | Paramètre manquant ou invalide / Non autorisé |
| 503 | Erreur base de données temporaire |

## Rate limiting

Les endpoints sont protégés par un rate limit horaire et journalier. En cas de dépassement, la réponse retourne un code `429`.
