```yaml
number: 580
title: F821 false positive when assigning to a dict
type: issue
state: closed
author: jiwidi
labels:
  - bug
assignees: []
created_at: 2022-11-04T11:55:05Z
updated_at: 2022-11-04T14:52:27Z
url: https://github.com/astral-sh/ruff/issues/580
synced_at: 2026-01-12T15:54:40Z
```

# F821 false positive when assigning to a dict

---

_@jiwidi_

Hi! Love what you are building with ruff. 

I'm trying it out now and found a weird behavior that i dont know if its a bug or something im missing.

```python
def api_event_json(api_key, model_name, user_id=None, item_id=None):
    dict = {
        "headers": {
            "x-api-key": api_key,
        },
        "queryStringParameters": {"limit": "5"},
        "pathParameters": {"model": model_name},
    }

    if user_id is not None:
        dict["queryStringParameters"]["user_id"] = user_id
    if item_id is not None:
        dict["queryStringParameters"]["item_id"] = item_id

    return dict
```

Why would this code throw F821? 

```
F821 Undefined name `queryStringParameters`
F821 Undefined name `queryStringParameters`
```

For the lines where we access the dict.

I can walk around this error by rearranging the code a bit:
```python
def api_event_json(api_key, model_name, user_id=None, item_id=None):

    query_params = {"limit": 5}
    if user_id is not None:
        query_params["user_id"] = user_id
    if item_id is not None:
        query_params["item_id"] = item_id

    dict = {
        "headers": {
            "x-api-key": api_key,
        },
        "queryStringParameters": query_params,
        "pathParameters": {"model": model_name},
    }

    return dict
```

And is alright. Wondering if its a bug.

---

_Comment by @charliermarsh on 2022-11-04 13:35_

Definitely a bug! It’s treating it like a type annotation with a forward reference (eg, x: dict[“foo”] = {}, which would be referencing a variable named “foo”). But it should only be enforcing that behavior for annotations. Will fix today.


---

_Label `bug` added by @charliermarsh on 2022-11-04 13:36_

---

_Renamed from "F821 Weird failure" to "F821 false positives when assigning to a dict" by @charliermarsh on 2022-11-04 13:43_

---

_Renamed from "F821 false positives when assigning to a dict" to "F821 false positive when assigning to a dict" by @charliermarsh on 2022-11-04 13:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-04 14:07_

---

_Comment by @charliermarsh on 2022-11-04 14:23_

Another way to fix this is to use a name other than `dict`. Part of the problem is that the code is shadowing the built-in `dict`.


---

_Closed by @charliermarsh on 2022-11-04 14:51_

---

_Comment by @charliermarsh on 2022-11-04 14:52_

This'll go out in the next release, later today!

---
