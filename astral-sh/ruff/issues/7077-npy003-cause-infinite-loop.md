```yaml
number: 7077
title: NPY003 cause infinite loop 
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T09:38:22Z
updated_at: 2023-09-03T15:53:29Z
url: https://github.com/astral-sh/ruff/issues/7077
synced_at: 2026-01-12T15:54:46Z
```

# NPY003 cause infinite loop 

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)

```
ruff --fix *.py  --no-cache --select NPY003
```

file content:
```
from numpy import alltrue as all
def _split(string, char=','):
        return []
class Sofd(table.Table):
    FILENAME = os.path.join(templates.get_template_dir(),
                    '{ins}', '{wkf}', 'sofd_{ins}_{wkf}_{step}.dat')
    def _complement_dataset(self, dataset, datalog):
                keep = all([sublog[g] == frame0[g] for g in group_by], axis=0)
```

error:
```
error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `588514.py`, the rule codes NPY003, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

588514.py:8:24: NPY003 `np.alltrue` is deprecated; use `np.all` instead
Found 101 errors (100 fixed, 1 remaining).

```

[588514.py.zip](https://github.com/astral-sh/ruff/files/12505641/588514.py.zip)


---

_Comment by @charliermarsh on 2023-09-03 12:00_

Haha that's such a brutal one.

---

_Label `bug` added by @charliermarsh on 2023-09-03 12:00_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 12:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 15:36_

---

_Closed by @charliermarsh on 2023-09-03 15:53_

---
