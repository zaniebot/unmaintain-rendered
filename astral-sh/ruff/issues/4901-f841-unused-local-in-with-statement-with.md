```yaml
number: 4901
title: "F841: unused local in 'with' statement with parethesised value does not converge"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-06T09:56:03Z
updated_at: 2023-06-06T20:15:23Z
url: https://github.com/astral-sh/ruff/issues/4901
synced_at: 2026-01-10T11:09:47Z
```

# F841: unused local in 'with' statement with parethesised value does not converge

---

_Issue opened by @addisoncrump on 2023-06-06 09:56_

This is a really specific one. The following snippet prevents Ruff from converging:

```py
def e():
  with (input('>')) as f:
    print(1)
```

To trigger this, the value specified in the `with` must be parethesised and the `with` must not be in the global scope of the script.

Discovered by #4822.

---

_Label `bug` added by @charliermarsh on 2023-06-06 13:25_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-06 14:42_

---

_Closed by @charliermarsh on 2023-06-06 20:15_

---
