```yaml
number: 2302
title: "Implement `TYP001` from `flake8-typing-imports`"
type: issue
state: open
author: charliermarsh
labels:
  - rule
assignees: []
created_at: 2023-01-28T16:40:47Z
updated_at: 2024-05-15T05:56:12Z
url: https://github.com/astral-sh/ruff/issues/2302
synced_at: 2026-01-10T11:09:45Z
```

# Implement `TYP001` from `flake8-typing-imports`

---

_Issue opened by @charliermarsh on 2023-01-28 16:40_

This rule detects whether you've imported something from `typing` that's incompatible with your supported Python version.

See: #2096.

---

_Label `rule` added by @charliermarsh on 2023-01-28 16:40_

---

_Comment by @DanielYang59 on 2024-05-15 05:54_

+1. We do need a `ruff` rule as mentioned in #6650 that detects and guards `TYPE_CHECKING` only import :) Which also hopefully could improve runtime performance by not importing unnecessary packages.

Thanks a lot for your work!

---
