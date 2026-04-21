# Nolio Public Workout Format

This format expose structured workouts through the Nolio public API.  
A workout is represented as an **array of steps**, where each step can be one of the following types:  
- **atomic step**  
- **repetition**  
- **exercise**  
- **superset**  

---

## 1) Atomic step

| field               | type                              | optional | description                                                                 |
|---------------------|-----------------------------------|----------|-----------------------------------------------------------------------------|
| step_duration_type  | `"duration" \| "distance"`        | yes      | distance or duration                                                        |
| open_duration       | `boolean`                         | yes      |                                                 |
| step_duration_value | `number`                          | yes      | value in seconds or meters                                                  |
| target_value_max    | `number`                          | yes      | upper bound of the target                                                   |
| step_percent_low    | `number`                          | yes      | lower percent bound of the target                                                   |
| step_percent_high    | `number`                          | yes      | upper percent bound of the target                                                   |
| target_value_min    | `number`                          | yes      | lower bound of the target                                                   |
| rpe                 | `number`                          | yes      | rate of perceived exertion                   |
| rir                 | `number`                          | yes      | repetitions in reserve (only for Exercise)                                                      |
| manual_values                 | `boolean`                          | yes      | True when values are set manually|
| intensity_type      | `string`                          | yes      | normalized intensity                                   |
| target_type         | `string`                          | yes      | type of target: may be the `unit`, `"rir"`, `"rpe"`, or a metric (pace, hr) |
| name             | `string`                          | yes      | name of the target                                                                |
| secondary_step      | `atomic step`                     | yes      | secondary target if provided (e.g., cadence + power)                        |
| comment             | `string`                          | yes      | free note                                                                   |

---

## 2) Repetition

| field | type          | optional | description                      |
|-------|---------------|----------|----------------------------------|
| type  | `"repetition"`| no       | node type                        |
| value | `number`      | no       | number of repetitions            |
| steps | `array<step>` | no       | repeated content (any step type) |

---

## 3) Exercise

| field        | type          | optional | description        |
|--------------|---------------|----------|--------------------|
| type         | `"exercise"`  | no       | node type          |
| name         | `string`      | yes      | exercise name      |
| instructions | `string`      | yes      | exercise notes     |
| thumbnail_url| `string`      | yes      | thumbnail image    |
| media_url    | `string`      | yes      | media (e.g., video)|
| sets         | `array<set>`  | no       | exercise sets      |

### Set (inside `exercise.sets`)

| field       | type                               | optional | description                         |
|-------------|------------------------------------|----------|-------------------------------------|
| type        | `"rep" \| "distance" \| "duration"`| no       | set type                            |
| rest        | `number`                           | yes      | rest time in seconds                 |
| targets     | `array<atomic step>`               | no       | 1 or 2 targets                      |
| repetitions | `number`                           | yes      |                    |
| distance    | `number`                           | yes      | in meters  |
| duration    | `number`                           | yes      | in seconds |
| tempo       | `string`                           | yes      | tempo notation (e.g., `"3-1-2"`)     |

---

## 4) Superset

| field     | type              | optional | description           |
|-----------|-------------------|----------|-----------------------|
| type      | `"superset"`      | no       | node type             |
| exercises | `array<exercise>` | no       | list of exercises     |






***




**Format type: json**

Example 1
``` json
{
  "structured_workout": [
    {
      "intensity_type": "warmup",
      "step_duration_type": "duration",
      "step_duration_value": 600,
      "type": "step",
      "target_type": "power",
      "target_value_min": 200,
      "target_value_max": 250,
      "open_duration": true,
      "comment": "Hello world\nFrom API"
    },
    {
      "type": "repetition",
      "value": 3,
      "steps": [
        {
          "intensity_type": "active",
          "step_duration_type": "duration",
          "step_duration_value": 180,
          "type": "step",
          "target_type": "power",
          "target_value_min": 300,
          "target_value_max": 350
        },
        {
          "intensity_type": "rest",
          "step_duration_type": "duration",
          "step_duration_value": 180,
          "type": "step",
          "target_type": "no_target"
        }
      ]
    },
    {
      "intensity_type": "cooldown",
      "step_duration_type": "distance",
      "step_duration_value": 5000,
      "type": "step",
      "target_type": "heartrate",
      "target_value_min": 100,
      "target_value_max": 150,
      "open_duration": true
    }
  ]
}
```

1. Valid Enum Values
   * intensity_type: 
     * warmup
     * cooldown
     * active
     * rest
     * ramp_up
     * ramp_down

   * step_duration_type:
     * duration
     * distance

   * type:
     * repetition
     * step

   * target_type
     * pace
     * speed
     * power
     * heartrate
     * no_target

2. Units

|||
|----------|:-------------:|
| step_duration_value |  meter or second |
| heartrate |    bpm   |
| power | W |
| speed | km/h |
| pace | mps (meters/s)|
| cadence | rpm |

3. Notes

   * **step_duration_value** values are integers.
   * If target_type is not "no_target", target_value_max is mandatory, but target_value_min is optional. If both target_value_min and target_value_max are completed, target will be a range of [target_value_min, target_value_max], otherwise it will be a target of target_value_max
   * For open duration steps you can add "open_duration" : true to a step.
   * You can add linebreaks on the comment field with \n

     




 


