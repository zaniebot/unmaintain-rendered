```yaml
number: 9491
title: False positive in 0.1.12 PD002 with pyvista inplace
type: issue
state: closed
author: tarrox
labels:
  - bug
assignees: []
created_at: 2024-01-12T12:35:56Z
updated_at: 2024-01-12T19:12:55Z
url: https://github.com/astral-sh/ruff/issues/9491
synced_at: 2026-01-12T15:54:49Z
```

# False positive in 0.1.12 PD002 with pyvista inplace

---

_@tarrox_

Problem:
Pyvista supports to change meshes inplace, which triggers the PD002 rule for pandas.

Ruff Version: ruff 0.1.12

Dependencies:

* pyvista: 0.43.1

Config:
```
line-length = 100
ignore = [
    "F722",
    "G004",
]
select = [
    "E","F","W","N","D","I","G","T","PT","RET","PTH","FIX","PD","RUF","YTT","BLE","B","A","EXE","SLF","PIE",
]
```

```python
"""Nothing."""
from pyvista import examples


def main():
    """Nothing."""
    mesh = examples.download_crater_topo()
    mesh.rotate_z(45, inplace=True)

    mesh.plot()

if __name__ == "__main__":
    main()

```

---

_Label `bug` added by @charliermarsh on 2024-01-12 16:29_

---

_Comment by @charliermarsh on 2024-01-12 16:29_

We can, at the very least, curate a list of methods that should be eligible to trigger this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-12 18:46_

---

_Closed by @charliermarsh on 2024-01-12 19:12_

---
