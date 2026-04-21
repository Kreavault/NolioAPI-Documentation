# Research Project API Documentation

## Overview

The "Research Project" API is designed to manage participants, trainings, competitions, and metrics in a sports research context. It offers functionalities for creating, retrieving, updating, and deleting information related to these domains.

## Security and Authentication

To interact with the "Research Project" API, it is necessary to include an authentication header in every request. This mechanism ensures that only authorized users can access the API features.

### API Secret Key

Each request to the API must include an `Authorization` header containing the research team's secret key. This process is managed by the `research_token` decorator, which validates the secret key for each request.

#### Example of Authentication Header:

```bash
curl -X GET https://www.nolio.io/api/research/participants/ \
    -H "Authorization: Bearer YOUR_SECRET_KEY"
```


### Participants
- **GET `/api/research/participants/`**  
  Retrieves the list of participants.

### Groups

- **`GET /groups/`**

```json
{
  "groups": [
    {
      "id": 1,
      "name": "Control",
      "participants": [123, 456]
    }
  ]
}
```

- **`POST /update/groups/`**

Assign / remove users in bulk. After the API call, `users` will be in and only in `groups`

Exemple of body of POST to send

```json
{
  "users": [123,456],   // required – participant IDs
  "groups": [1,3]       // required – group IDs
}
```

Returns **201** with empty body.

---

### Create message

- **POST `/create/message/`** 

Send messages from the team manager to athletes or groups.

Body parameters:

| Field               | Type   | Required | Notes                      |
| ------------------- | ------ | -------- | -------------------------- |
| `content_message`   | string | ✓        | Message body               |
| `read_only`         | bool   |          | Defaults to `false`        |
| `send_individually` | bool   |          | `true` → 1 thread per user |
| `ids_to`            | int[]  | (1)      | Participant IDs            |
| `ids_group_to`      | int[]  | (1)      | Group IDs                  |

*(1) Exactly **one** of `ids_to` or `ids_group_to` must be provided.*

Returns **201** empty body.

### Training and Competition
- **GET `/api/research/<training|competition>/<int:user_id>/`**  
  Retrieves trainings for a specific user.

- **GET `/api/research/training/<int:pk>/streams/`**  
  Retrieves data streams for a specific training.

### Notes
- **GET `/api/research/notes/<int:user_id>/`**  
  Retrieves notes for a specific user.

### Notes planned
- **GET `/api/research/planned/notes/<int:user_id>/`**  
  Retrieves notes planned for a specific user.


### Metrics
- **GET `/api/research/metrics/<int:user_id>/`**  
  Retrieves metrics for a specific user.

### Planned Training and Competition
- **GET `/api/research/planned/<training|competition>/<int:user_id>/`**  
  Retrieves trainings planned for a specific user.


- **POST `/api/research/create/<training|competition>/planned/<int:user_id>/`**
  Creates a planned training or competition for a user.  
  - **Body Parameters:**  
    - `id_partner`: Integer, required.  
    - `name`: String, required.  
    - `sport_id`: Integer, required.  
    - `date_start`: Date in YYYY-MM-DD format, required.  
    - `plan_id`: Integer, optional.

- **POST `/api/research/update/<training|competition>/planned/<int:user_id>/`**
  Updates a planned training for a user.  
  - **Body Parameters:** Similar to create endpoint.

- **POST `/api/research/delete/<training|competition>/planned/<int:user_id>/`**
  Deletes a planned training for a user.  
  - **Body Parameters:**  
    - `id_partner`: Integer, required.

### Training Plans
- **POST `/api/research/create/trainingplan/`**  
  Creates a training plan.  
  - **Body Parameters:**  
    - `name`: String, required.

- **POST `/api/research/update/trainingplan/<int:user_id>/<int:plan_id>/`**  
  Updates a training plan.  
  - **Body Parameters:**  
    - `name`: String, required.

- **POST `/api/research/delete/trainingplan/<int:user_id>/<int:plan_id>/`**  
  Deletes a training plan.

Note: A training plan allows grouping several planned training sessions under the same entity. It starts with creating a plan, then injecting the plan's ID into the desired planned sessions using the planned session update endpoint. Deleting a training plan will remove all the planned sessions associated with that plan.

### Create a planned workout
To create a planned workout, you can use the following `curl` command:

```bash
curl -X POST https://www.nolio.io/api/research/create/training/planned/<user_id>/ \
    -H "Content-Type: application/json" \
    -d '{
        "id_partner": 42,
        "name": "Workout name",
        "sport_id": 23,
        "date_start": "2023-12-01",
        "plan_id": 42
    }'
```

## Available Metric Types

The API supports a variety of metric types for detailed tracking and analysis. Below is a list of the available metric types:

### Body Metrics
- **Weight**: Reflects the body weight of the user.
- **Fat Content**: Indicates the body fat percentage.

### Heart Rate Metrics
- **Max Heart Rate**: The maximum heart rate achieved.
- **Max Heart Rate (Bike)**: The maximum heart rate achieved specifically during biking.
- **Resting Heart Rate**: The heart rate when the user is at rest.
- **Heart Rate Recovery (HRR)**: Measures the rate of recovery after exercise.
- **RMSSD**: Root Mean Square of Successive Differences, a measure of heart rate variability.

### Performance Metrics
- **Maximal Aerobic Speed (VMA)**: Indicates the aerobic capacity during intense exercise.
- **VO2 Max**: The maximum rate of oxygen consumption measured during incremental exercise.
- **Maximal Aerobic Power (PMA)**: Indicates the aerobic power.
- **Functional Threshold Power (FTP)**: The highest power that a user can maintain in a steady state for an hour (for cycling).
- **Functional Threshold Power (Run)**: Similar to FTP but for running.

### Sleep Metrics
- **Sleep Duration**: Total duration of sleep.
- **Sleep Quality Score**: A score representing the overall quality of sleep.
- **Light Sleep Duration**: Duration of light sleep phase.
- **Deep Sleep Duration**: Duration of deep sleep phase.
- **REM Sleep Duration**: Duration of REM (Rapid Eye Movement) sleep phase.
- **Awake Duration**: Duration of time spent awake during sleep.

### Wellness Metrics
- **Body Battery Level**: Indicates the body's energy reserves.
- **Number of Recharges**: The frequency of recharge or rest periods.
- **Oura Readiness Score**: Readiness score from Oura Ring data.
- **Whoop Readiness Score**: Readiness score from Whoop data.
- **Nolio HRV Score**: Heart rate variability score calculated by Nolio.
