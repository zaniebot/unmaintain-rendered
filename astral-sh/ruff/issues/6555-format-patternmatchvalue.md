---
number: 6555
title: "Format `PatternMatchValue`"
type: issue
state: closed
author: konstin
labels:
  - good first issue
  - formatter
assignees: []
created_at: 2023-08-14T12:30:25Z
updated_at: 2023-08-23T14:01:17Z
url: https://github.com/astral-sh/ruff/issues/6555
synced_at: 2026-01-10T01:22:45Z
---

# Format `PatternMatchValue`

---

_Issue opened by @konstin on 2023-08-14 12:30_

Implement formatting for `PatternMatchValue`, see https://github.com/astral-sh/ruff/issues/5834

Implement formatting for the value pattern:

```python
match x:
    case "Relevant":
        ...
```

---

_Referenced in [astral-sh/ruff#5834](../../astral-sh/ruff/issues/5834.md) on 2023-08-14 12:30_

---

_Label `good first issue` added by @konstin on 2023-08-14 12:30_

---

_Label `formatter` added by @konstin on 2023-08-14 12:30_

---

_Comment by @dhruvmanila on 2023-08-14 12:39_

This could be delegated directly to the expression formatting. We probably just need to make sure that the comments are handled correctly around the pattern itself which might be handled automatically by the expression formatting itself.

---

_Referenced in [astral-sh/ruff#6557](../../astral-sh/ruff/issues/6557.md) on 2023-08-14 12:41_

---

_Referenced in [astral-sh/ruff#6558](../../astral-sh/ruff/issues/6558.md) on 2023-08-14 12:46_

---

_Added to milestone `Formatter: Alpha` by @dhruvmanila on 2023-08-14 13:41_

---

_Comment by @evanrittenhouse on 2023-08-14 18:59_

@MichaReiser I can take this

---

_Assigned to @evanrittenhouse by @dhruvmanila on 2023-08-15 01:51_

---

_Referenced in [astral-sh/ruff#6799](../../astral-sh/ruff/pulls/6799.md) on 2023-08-23 08:23_

---

_Closed by @charliermarsh on 2023-08-23 14:01_

---

_Closed by @charliermarsh on 2023-08-23 14:01_

---
