Create a message on a training

#### Endpoint:
  - **/create/training/message/**
  - HTTP Method: **POST**

Example:
```
Production:
https://www.nolio.io/api/create/training/message/
```

Send a message (comment/reaction) on an athlete's training. This creates a conversation thread linked to the training between the coach and the athlete. If a thread already exists for that training between both users, the message is added as a reply.

Parameters :

* content: string, required. The message text. Max 10,000 characters.
* id_training: integer, optional. The Nolio training ID. Either `id_training` or `id_partner` must be provided.
* id_partner: string, optional. The partner/external training ID (from your application). Either `id_training` or `id_partner` must be provided. The training must have been originally created by your OAuth application.
* athlete_id: integer, optional. If provided, the message will be sent on a training of the specified athlete (you must be their coach). You can retrieve athlete ids with the [Get Athletes](https://github.com/NolioApp/NolioAPI-Documentation/wiki/User-Get-Athletes) route.

Example:
```json
  {
    "id_training": 456,
    "athlete_id": 123,
    "content": "Great work on that interval session!"
  }
```

### Response
A JSON containing the created message data:

```json
  {
    "thread_id": 789,
    "message_id": 101,
    "is_new_thread": true,
    "training_id": 456
  }
```

* **thread_id**: integer. The conversation thread ID (new or existing).
* **message_id**: integer. The ID of the created message.
* **is_new_thread**: boolean. `true` if a new thread was created, `false` if the message was added to an existing thread.
* **training_id**: integer. The training ID.

### Specific status code

* 400 Bad request

  * `"content is required"` - content is missing or empty
  * `"content too long"` - content exceeds 10,000 characters
  * `"id_partner or id_training is required"` - neither training identifier provided
  * `"Invalid id_training"` - id_training is not a valid integer
  * `"Training does not exist"` - no training found for the given ID
  * `"coach cannot react to their own training"` - coach and athlete are the same user
