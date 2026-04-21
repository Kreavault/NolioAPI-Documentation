* [Training map](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#training-map)
* [Sport map](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#sport-map)
* [Feeling map](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#feeling-map)
* [RPE map](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Training-Object#rpe-map)

## Training map (for manual creation)

| Parameter      | Type    | Unit         | Required |
|----------------|---------|--------------|----------|
| id_partner     | Integer |              | true     |
| sport_id       | Integer |              | true     |
| name           | String  |              | true     |
| date_start     | Date    |              | true     |
| description    | String  |              |          |
| duration       | Integer | seconds      |          |
| sensation      | Integer | from 1 to 5  |          |
| rpe            | Integer | from 1 to 10 |          |
| distance       | Integer | kilometers   |          |
| elevation_gain | Integer | meters       |          |

## Training map (for retrieving)

| Parameter      | Type    | Unit         | Comment  |
|----------------|---------|--------------|----------|
| nolio_id       | Integer |              | can be used for other endpoint where <nolio_id> is required     |
| name           | String  |              |      |
| sport           | String  |              |      |
| sport_id           | Integer  |              | Nolio sport_id   |
| date_start     | Date    |              | YYYY-MM-DD format |
| hour_start     | Date    |              | HH:MM:SS format |
| description    | String  |              |          |
| duration       | Integer | seconds      |          |
| distance       | Float | kilometers   |          |
| rpe            | Integer | from 1 to 10 |          |
| feeling        | Integer | from 1 to 5  |          |
| elevation_gain | Integer | meters       |          |
| load_foster | Float |        |          |
| load_coggan | Float |        |          |
| rest_hr_user | Integer | bpm       | Workout user resting heartrate (not during the workout)        |
| max_hr_user | Integer | bpm       | Workout user maximum heartrate (not during the workout)      |
| np | Float | W       | Power adjusted         |
| ftp | Float | W       |          |
| rftp | Float | W       |          |
| weight | Float | kg   |  User weight     |
| critical_power | Float | W       |          |
| wbal | Float | J       |          |
| file_url | Url |        | Url to download the .fit or .tcx file associated to the workout, **valid only for one hour**         |
| planned_name | String |  |  Planned workout name        |
| planned_sport | String |  | Planned workout sport name         |
| planned_sport_id | Integer |  | Planned workout Nolio sport_id       |
| planned_description | String |  | Planned workout description       |
| planned_load_coggan | Float |  | Planned workout Coggan load       |
| is_competition | Bool |  | Is the workout flagged as competition or not       |


## Planned Training map (for retrieving)

| Parameter      | Type    | Unit         | Comment  |
|----------------|---------|--------------|----------|
| nolio_id       | Integer |              | can be used for other endpoint where <nolio_id> is required     |
| name           | String  |              |      |
| sport           | String  |              |      |
| sport_id           | Integer  |              | Nolio sport_id   |
| date_start     | Date    |              | YYYY-MM-DD format |
| hour_start     | Date    |              | HH:MM:SS format |
| description    | String  |              |          |
| duration       | Integer | seconds      |          |
| distance       | Float | kilometers   |          |
| rpe            | Integer | from 1 to 10 |          |
| elevation_gain | Integer | meters       |          |
| load_foster | Float |        |          |
| load_coggan | Float |        |          |
| is_competition | Bool |  | Is the workout flagged as competition or not       |
| structured_workout | [Structured_workout](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Structured-Workout) |        |          |


## Training map streams
| Parameter      | Type    | Unit         |
|----------------|---------|--------------|
| stream_heartrate | Float |  bpm | 
| stream_torque | Float |  N.m | 
| stream_watts | Float |  W | 
| stream_cadence | Float |  rpm or ppm | 
| stream_pace | Float |  meters per second | 
| stream_altitude | Float |  meters | 
| stream_distance | Float |  meters | 
| stream_time | Float |  seconds | 

## Sport map
| Nolio Sport          | Sport Id |
|----------------------|----------|
| XC ski - Classic     | 3        |
| XC ski - Skating     | 4        |
| Roller ski - Classic | 5        |
| Roller ski - Skating | 6        |
| Ski Mountaineering   | 7        |
| Climbing             | 8        |
| Bodybuilding         | 10       |
| Other                | 12       |
| Road cycling         | 14       |
| Mountain cycling     | 15       |
| Hiking               | 16       |
| Virtual ride         | 18       |
| Swimming             | 19       |
| Strength             | 20       |
| Stretching           | 21       |
| Running              | 2        |
| Treadmill            | 24       |
| Kayaking - Sea       | 26       |
| Kayaking - River     | 27       |
| Elliptical trainer   | 28       |
| Walking sticks       | 29       |
| Yoga                 | 30       |
| Canoe - Sea          | 31       |
| Canoe - River        | 32       |
| Rowing               | 33       |
| Orienteering race    | 34       |
| Track cycling        | 35       |
| CX cycling           | 36       |
| Squash               | 37       |
| Biathlon             | 38       |
| Walking              | 45       |
| Stand up paddle      | 51       |
| Trail running        | 52       |
| OCR running          | 53       |
| Tennis               | 59       |

## Feeling map

| 1 | Very weak |
|---|-----------|
| 2 | Weak      |
| 3 | Normal    |
| 4 | Good      |
| 5 | Strong    |

## Rpe map

| 1  | Very easy |
|----|-----------|
| 2  | Easy      |
| 3  | Easy      |
| 4  | Moderate  |
| 5  | Moderate  |
| 6  | Hard      |
| 7  | Hard      |
| 8  | Very hard |
| 9  | Very hard |
| 10 | All out   |