---
number: 6645
title: "Format `PatternMatchSequence`"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-17T09:51:37Z
updated_at: 2023-08-23T00:44:36Z
url: https://github.com/astral-sh/ruff/issues/6645
synced_at: 2026-01-07T13:12:15-06:00
---

# Format `PatternMatchSequence`

---

_Issue opened by @MichaReiser on 2023-08-17 09:51_

Implement formatting of `[a, b]` match sequences. 

```python
match x:
    case [1, 2]:
        ...
```

This should probably be similar to list formatting.

---

_Referenced in [astral-sh/ruff#5834](../../astral-sh/ruff/issues/5834.md) on 2023-08-17 09:51_

---

_Label `formatter` added by @MichaReiser on 2023-08-17 09:52_

---

_Label `help wanted` added by @MichaReiser on 2023-08-17 09:52_

---

_Referenced in [astral-sh/ruff#6653](../../astral-sh/ruff/pulls/6653.md) on 2023-08-17 15:34_

---

_Comment by @harupy on 2023-08-18 12:00_

@MichaReiser 

---

_Assigned to @harupy by @MichaReiser on 2023-08-18 12:23_

---

_Referenced in [astral-sh/ruff#6676](../../astral-sh/ruff/pulls/6676.md) on 2023-08-18 14:39_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:14_

---

_Closed by @charliermarsh on 2023-08-23 00:44_

---
