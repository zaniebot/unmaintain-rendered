```yaml
number: 10310
title: "`RUF001` isn't highlighted for implicitly concatenated f-string"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - linter
assignees: []
created_at: 2024-03-09T11:21:06Z
updated_at: 2024-03-14T08:00:24Z
url: https://github.com/astral-sh/ruff/issues/10310
synced_at: 2026-01-10T11:09:52Z
```

# `RUF001` isn't highlighted for implicitly concatenated f-string

---

_Issue opened by @dhruvmanila on 2024-03-09 11:21_

Given:

```python
x = '𝐁ad' f'𝐁ad string'
#           ^
#           Ruff only highlights this
```

Ruff only highlights the bad character from the second part of the implicitly concatenated string.

https://play.ruff.rs/16071c4c-a1dd-4920-b56f-e2ce2f69c843

---

_Label `bug` added by @dhruvmanila on 2024-03-09 11:21_

---

_Label `linter` added by @zanieb on 2024-03-11 16:52_

---

_Closed by @dhruvmanila on 2024-03-14 08:00_

---
