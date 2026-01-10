```yaml
number: 13759
title: "Ruff doesn't raise syntax error for single starred assignment target"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2024-10-15T10:21:28Z
updated_at: 2025-03-29T16:35:49Z
url: https://github.com/astral-sh/ruff/issues/13759
synced_at: 2026-01-10T11:09:55Z
```

# Ruff doesn't raise syntax error for single starred assignment target

---

_Issue opened by @dhruvmanila on 2024-10-15 10:21_

We should be raising syntax error for the following code:

```py
*a = (1,)
```

https://play.ruff.rs/7e3fe1a8-7edc-40eb-a415-ced8f61bb7c1

---

_Label `bug` added by @dhruvmanila on 2024-10-15 10:21_

---

_Comment by @dhruvmanila on 2024-10-15 10:44_

Oh, I think this is caught by the compiler and not the parser as `python -m ast test.py` passes without any syntax errors. So, this is a part of #11934.

---

_Closed by @ntBre on 2025-03-29 16:35_

---
