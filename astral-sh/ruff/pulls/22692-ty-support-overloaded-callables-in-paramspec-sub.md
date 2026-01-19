```yaml
number: 22692
title: "[ty] Support overloaded callables in `ParamSpec` sub-call evaluation"
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: charlie/param-call
created_at: 2026-01-19T00:55:15Z
updated_at: 2026-01-19T00:55:56Z
url: https://github.com/astral-sh/ruff/pull/22692
synced_at: 2026-01-19T01:23:17Z
```

# [ty] Support overloaded callables in `ParamSpec` sub-call evaluation

---

_@charliermarsh_

## Summary

Previously, when an overloaded function was passed to a function using `ParamSpec` (like `asyncio.to_thread()`), ty would fail to perform overload resolution and emit spurious `invalid-argument-type` errors.

Closes https://github.com/astral-sh/ty/issues/1838.


---

_Label `bug` added by @charliermarsh on 2026-01-19 00:55_

---

_Label `ty` added by @charliermarsh on 2026-01-19 00:55_

---

_Closed by @charliermarsh on 2026-01-19 00:55_

---
