```yaml
number: 2591
title: Make RSE102 autofixable
type: issue
state: closed
author: LefterisJP
labels:
  - fixes
assignees: []
created_at: 2023-02-05T22:10:31Z
updated_at: 2023-02-05T23:19:28Z
url: https://github.com/astral-sh/ruff/issues/2591
synced_at: 2026-01-12T15:54:43Z
```

# Make RSE102 autofixable

---

_@LefterisJP_

With ruff 0.0.241 I checked the `RSE102` rule.

It's `RSE102 Unnecessary parentheses on raised exception`. So it should be really easy to just autofix.

`raise NoArgsException()` -> `raise NoArgsException`

---

_Label `autofix` added by @charliermarsh on 2023-02-05 22:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-05 22:30_

---

_Closed by @charliermarsh on 2023-02-05 23:19_

---
