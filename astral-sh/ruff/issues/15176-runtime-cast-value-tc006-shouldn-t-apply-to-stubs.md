```yaml
number: 15176
title: "`runtime-cast-value (TC006)` shouldn't apply to stubs"
type: issue
state: closed
author: Avasam
labels: []
assignees: []
created_at: 2024-12-29T01:36:43Z
updated_at: 2024-12-29T19:39:17Z
url: https://github.com/astral-sh/ruff/issues/15176
synced_at: 2026-01-12T15:54:54Z
```

# `runtime-cast-value (TC006)` shouldn't apply to stubs

---

_@Avasam_

Found this whilst applying various Ruff groups on typeshed.

According to the updated typing spec for typing enums (see [Mypy: Change to Enum Membership Semantics](https://mypy-lang.blogspot.com/2024/12/mypy-114-released.html) and https://typing.readthedocs.io/en/latest/spec/enums.html#defining-members, this is the recommended way to type an enum in stubs where the type is known but the value isn't:
```py
class Key(enum.Enum):
    alt = cast(KeyCode, ...)
    alt_l = cast(KeyCode, ...)
    alt_r = cast(KeyCode, ...)
    alt_gr = cast(KeyCode, ...)
    # ...
```
This will trigger `runtime-cast-value (TC006)`

Related: https://github.com/astral-sh/ruff/issues/15132

Ruff: `0.8.4`
Command: `ruff check --preview --select=TC006 --isolated`

---

_Comment by @Daverball on 2024-12-29 07:32_

My bad, some of the other new `flake8-type-checking` rules I added might also currently be active in stub files. None of them should really be active.

---

_Closed by @charliermarsh on 2024-12-29 19:39_

---

_Closed by @charliermarsh on 2024-12-29 19:39_

---
