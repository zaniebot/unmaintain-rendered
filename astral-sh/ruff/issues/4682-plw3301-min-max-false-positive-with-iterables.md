```yaml
number: 4682
title: "PLW3301: min/max false positive with iterables"
type: issue
state: closed
author: lovetox
labels:
  - bug
assignees: []
created_at: 2023-05-27T15:46:23Z
updated_at: 2023-05-27T19:34:57Z
url: https://github.com/astral-sh/ruff/issues/4682
synced_at: 2026-01-10T11:09:47Z
```

# PLW3301: min/max false positive with iterables

---

_Issue opened by @lovetox on 2023-05-27 15:46_

ruff: 0.0.267

```
tuples_list = [
    (1, 2),
    (2, 3),
    (3, 4),
    (4, 5),
    (5, 6),
]

max(max(tuples_list))
```

This raises `PLW3301 [*] Nested `max` calls can be flattened`

i don’t see how this can be flattend, if you only call max() once, it will return a tuple instead of the integer

---

_Renamed from "min/max false positive with iterables" to "PLW3301: min/max false positive with iterables" by @lovetox on 2023-05-27 15:46_

---

_Label `bug` added by @charliermarsh on 2023-05-27 19:17_

---

_Comment by @charliermarsh on 2023-05-27 19:17_

Thanks, you're right :)

---

_Comment by @charliermarsh on 2023-05-27 19:18_

Interestingly, Pylint flags this too.

---

_Closed by @charliermarsh on 2023-05-27 19:34_

---
