```yaml
number: 4287
title: TRY301 flags try - raise - finally block
type: issue
state: closed
author: jankatins
labels:
  - bug
assignees: []
created_at: 2023-05-08T17:29:45Z
updated_at: 2023-05-09T09:40:52Z
url: https://github.com/astral-sh/ruff/issues/4287
synced_at: 2026-01-12T15:54:44Z
```

# TRY301 flags try - raise - finally block

---

_@jankatins_

[TRY301](https://beta.ruff.rs/docs/rules/raise-within-try/) should flag cases like the below where the same code raises and catches an exception:

```py
try:
     raise Exception()
except:
    .... directly catch the exception
```

On the other hand, the following code is IMO a valid pattern (which would be better handled by a context manager, but in this case, my Consumer does not implement the context manager protocol):

```py
c = confluent_kafka.Consumer(...)
c.subscribe(["topic"])
try:
    # Wait a maximum of 5s for a message: we are doing this in tests
    msg = c.poll(5)
    if msg is None:
        # The next line is flagged with TRY301 Abstract `raise` to an inner function
        raise RuntimeError("Didn't find a message after 5 seconds")
    if msg.error():
        # The next line is flagged with TRY301 Abstract `raise` to an inner function
        raise RuntimeError(f"Error: {msg.error()}")
finally:
    c.close()
```

So from my perspective, the rule should check if there is actually a catching except block...

---

_Comment by @charliermarsh on 2023-05-09 02:27_

I think that's reasonable.

---

_Label `bug` added by @charliermarsh on 2023-05-09 02:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-09 03:31_

---

_Closed by @charliermarsh on 2023-05-09 03:38_

---

_Comment by @jankatins on 2023-05-09 09:40_

Thank you!

---
