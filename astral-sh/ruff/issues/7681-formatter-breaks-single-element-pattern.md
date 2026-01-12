```yaml
number: 7681
title: "Formatter: breaks single-element pattern"
type: issue
state: closed
author: mrcljx
labels:
  - bug
  - formatter
  - accepted
assignees: []
created_at: 2023-09-27T21:34:36Z
updated_at: 2023-09-27T23:57:19Z
url: https://github.com/astral-sh/ruff/issues/7681
synced_at: 2026-01-12T15:54:47Z
```

# Formatter: breaks single-element pattern

---

_@mrcljx_

A `case` with single-element tuple pattern (e.g. `(x,)`) is broken into multiple lines:

```
match a:
    case (x,):
        pass
```

is formatted to

```
match f:
    case (
        x,
    ):
        pass
```

Would have expected the formatter leaving it alone like it does for `(x,) = y` (and like Black does).

### Invocation

`ruff format test.py`

### Version

ruff 0.0.291


---

_Label `bug` added by @charliermarsh on 2023-09-27 22:05_

---

_Label `formatter` added by @charliermarsh on 2023-09-27 22:05_

---

_Comment by @charliermarsh on 2023-09-27 22:05_

Thanks! I consider that a bug too, since it deviates from our `if (x,):` handling.

---

_Added to milestone `Formatter: Stable` by @charliermarsh on 2023-09-27 22:06_

---

_Label `accepted` added by @charliermarsh on 2023-09-27 22:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 22:39_

---

_Closed by @charliermarsh on 2023-09-27 23:57_

---
