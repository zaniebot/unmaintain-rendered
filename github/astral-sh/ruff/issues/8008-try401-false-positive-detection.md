---
number: 8008
title: TRY401 false-positive detection
type: issue
state: closed
author: a-shchupakov
labels:
  - bug
assignees: []
created_at: 2023-10-17T11:17:37Z
updated_at: 2024-02-09T22:24:07Z
url: https://github.com/astral-sh/ruff/issues/8008
synced_at: 2026-01-07T13:12:15-06:00
---

# TRY401 false-positive detection

---

_Issue opened by @a-shchupakov on 2023-10-17 11:17_

TRY401 false-positively triggered if `error` object passed to function call inside of `logger.exception` arguments

```python
try:
    pass
except SlackApiError as api_error:
    logger.exception(
        "Got %s error while posting comment to thread %s in channel %s",
        get_error_message(api_error),  # <--- TRY401
        thread_ts,
        channel_name,
    )
    raise
```

`get_error_message` returns some string from `error`

* Playground link - https://play.ruff.rs/cad4767c-2a7a-4a77-be41-332d36e42817
* `ruff 0.1.0`.

---

_Comment by @charliermarsh on 2023-10-17 13:55_

We can fix this, but we'll be trading off false positives for false negatives. I guess it's much more likely that something intentional is happening to extract the error in the function call than anything else though.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-17 13:57_

---

_Label `bug` added by @charliermarsh on 2023-10-17 13:57_

---

_Comment by @a-shchupakov on 2023-10-17 14:42_

Thank you for response!
I moved it into variable and error is gone

So if you think current behaviour is fine, let's close it or wait for more feedback from other users :)

---

_Closed by @charliermarsh on 2023-10-17 15:08_

---

_Comment by @danhje on 2024-02-09 22:24_

Encountered exactly the same. I would guess `logger.exception(get_exception_message(e))` is quite a common pattern.

---
