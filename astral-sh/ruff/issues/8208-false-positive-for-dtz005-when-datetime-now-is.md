```yaml
number: 8208
title: "False positive for DTZ005 when `datetime.now()` is used with `astimezone`"
type: issue
state: closed
author: smurfix
labels: []
assignees: []
created_at: 2023-10-25T10:25:50Z
updated_at: 2023-10-25T10:40:37Z
url: https://github.com/astral-sh/ruff/issues/8208
synced_at: 2026-01-12T15:54:47Z
```

# False positive for DTZ005 when `datetime.now()` is used with `astimezone`

---

_@smurfix_

`t = datetime.now().astimezone()`

This reports DTZ005. Unfortunately, this is the most efficient way to get the current time in the current timezone.

Thus, please recognize this idiom and don't warn.

Version: ruff 0.1.1

---

_Comment by @smurfix on 2023-10-25 10:32_

ditto for DTZ006; DTZ007 on the other hand handles this correctly.

---

_Closed by @smurfix on 2023-10-25 10:40_

---

_Comment by @smurfix on 2023-10-25 10:40_

Already works. My mistake.

---
