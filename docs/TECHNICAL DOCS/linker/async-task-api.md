---
title: The Sefaria Linker Async Task API
excerpt: Learn how to use the Sefaria Linker Async Task API
deprecated: false
hidden: false
metadata:
  robots: index
---
The Async Task API can respond with the status of any async task. If the task is complete, it also responds with the result payload of the task. Currently the only async task is the [Linker API](doc:linker-api)

# GET /api/async/:task_id

Takes a task_id as returned by the original API that generated the task. E.g. a task ID is returned when the linker API is called.

## Response Format

```javascript
{
  "task_id": <str>,  # same as task ID sent in the request
  "state": <str>,    # see below
  "ready": <bool>    # true if result is ready
  "result": <dict>   # only appears when result is ready, contains payload of task result
  "error": <str>     # error message if the task failed
}
```

Below is an explanation of states that are possible:

| **State**  | **Description**                                                                             |
| ---------- | ------------------------------------------------------------------------------------------- |
| `PENDING`  | The task is waiting for execution or unknown (e.g., the result isn’t in the backend yet).   |
| `RECEIVED` | The worker has received the task and will start executing it soon.                          |
| `STARTED`  | The task has begun execution. (Only set if `task_track_started=True` in the Celery config.) |
| `RETRY`    | The task failed but will be retried (due to a raised `Retry` exception).                    |
| `SUCCESS`  | The task executed successfully and returned a result.                                       |
| `FAILURE`  | The task raised an exception during execution.                                              |