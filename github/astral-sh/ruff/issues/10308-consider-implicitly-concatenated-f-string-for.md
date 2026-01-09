---
number: 10308
title: Consider implicitly concatenated f-string for rules
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - linter
assignees: []
created_at: 2024-03-09T10:51:07Z
updated_at: 2024-03-11T16:53:39Z
url: https://github.com/astral-sh/ruff/issues/10308
synced_at: 2026-01-07T13:12:15-06:00
---

# Consider implicitly concatenated f-string for rules

---

_Issue opened by @dhruvmanila on 2024-03-09 10:51_

Certain rules which look at f-strings, especially implicitly concatenated ones, performs the check for an individual string element and not the concatenated one. Taking https://docs.astral.sh/ruff/rules/hardcoded-bind-all-interfaces/ as an example,

```python
# We don't highlight this but we used to before the implicit string concatenation refactor
'0.0.' f'0.0{expr}'

# We highlight the ones marked by `^`
'0.0.0.0' f'0.0.0.0{expr}0.0.0.0'
#           ^^^^^^^      ^^^^^^^
```

This is different for strings which are implicitly concatenated. For example,

```
src/S.py:2:1: S104 Possible binding to all interfaces
  |
1 | '0.0.0.0' f'0.0.0.0{expr}0.0.0.0'
2 | '0.0.' '0.0'
  | ^^^^^^^^^^^^ S104
  |
```

The reason we used to highlight the first example before the string refactor is that we would normalize the implicitly concatenated f-strings. So, the above example would become `f"0.0.0.0{expr}"`.

We should add some logic to get the concatenated strings for an implicitly concatenated f-string.

---

_Label `bug` added by @dhruvmanila on 2024-03-09 10:51_

---

_Label `linter` added by @zanieb on 2024-03-11 16:53_

---

_Referenced in [astral-sh/ruff#10311](../../astral-sh/ruff/pulls/10311.md) on 2024-03-13 08:44_

---

_Referenced in [astral-sh/ruff#10651](../../astral-sh/ruff/pulls/10651.md) on 2024-03-29 03:20_

---
