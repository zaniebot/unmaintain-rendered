```yaml
number: 2878
title: "Suggestion: Do not autofix TRY004 `prefer-type-error`"
type: issue
state: closed
author: adamtheturtle
labels:
  - fixes
assignees: []
created_at: 2023-02-14T00:57:36Z
updated_at: 2023-02-14T02:26:24Z
url: https://github.com/astral-sh/ruff/issues/2878
synced_at: 2026-01-12T15:54:43Z
```

# Suggestion: Do not autofix TRY004 `prefer-type-error`

---

_@adamtheturtle_

This can change functionality as the changed call may have callers which catch and handle `ValueError`.

---

_Label `autofix` added by @charliermarsh on 2023-02-14 02:16_

---

_Comment by @charliermarsh on 2023-02-14 02:16_

Yeah I agree with this.

---

_Closed by @charliermarsh on 2023-02-14 02:26_

---
