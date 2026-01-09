---
number: 6644
title: "Format `PatternMatchMapping`"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-17T09:51:34Z
updated_at: 2023-08-25T04:33:36Z
url: https://github.com/astral-sh/ruff/issues/6644
synced_at: 2026-01-07T13:12:15-06:00
---

# Format `PatternMatchMapping`

---

_Issue opened by @MichaReiser on 2023-08-17 09:51_

Implement formatting for `{...}` patterns

```python
match x:
    case {1: _, 2: _}:
        ...
    case {**rest}:
        ..
```

This should probably be similar to dict formatting.

---

_Referenced in [astral-sh/ruff#5834](../../astral-sh/ruff/issues/5834.md) on 2023-08-17 09:51_

---

_Label `formatter` added by @MichaReiser on 2023-08-17 09:52_

---

_Label `help wanted` added by @MichaReiser on 2023-08-17 09:52_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-24 01:55_

---

_Referenced in [astral-sh/ruff#6836](../../astral-sh/ruff/pulls/6836.md) on 2023-08-24 04:36_

---

_Closed by @charliermarsh on 2023-08-25 04:33_

---
