```yaml
number: 8282
title: Extend bad-dunder-method name to permit __index__
type: issue
state: closed
author: burgholzer
labels:
  - bug
assignees: []
created_at: 2023-10-27T12:33:30Z
updated_at: 2023-10-28T11:15:32Z
url: https://github.com/astral-sh/ruff/issues/8282
synced_at: 2026-01-12T15:54:48Z
```

# Extend bad-dunder-method name to permit __index__

---

_@burgholzer_

https://docs.astral.sh/ruff/rules/bad-dunder-method-name/ currently flags the standard python dunder method `__index__` (see https://docs.python.org/3.12/library/operator.html#operator.__index__ ) as invalid.

If you deem it reasonable, I am happy to submit a PR adding it.

---

_Comment by @charliermarsh on 2023-10-27 12:58_

That seems reasonable to me — PR welcome!

---

_Label `bug` added by @charliermarsh on 2023-10-27 13:38_

---

_Closed by @charliermarsh on 2023-10-28 11:15_

---
