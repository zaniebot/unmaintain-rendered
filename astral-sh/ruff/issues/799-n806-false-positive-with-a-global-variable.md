```yaml
number: 799
title: N806 false-positive with a global variable
type: issue
state: closed
author: actionless
labels:
  - bug
assignees: []
created_at: 2022-11-17T21:32:55Z
updated_at: 2023-01-27T13:10:04Z
url: https://github.com/astral-sh/ruff/issues/799
synced_at: 2026-01-12T15:54:40Z
```

# N806 false-positive with a global variable

---

_@actionless_

```python
GLOBAL_VAR: str = 'foo'

def get_all_aur_names() -> str:
    global GLOBAL_VAR
    if not GLOBAL_VAR:
        GLOBAL_VAR = 'bar'
    return GLOBAL_VAR
```

```console
$ ruff ruff_global_N806.py
Found 1 error(s).
ruff_global_N806.py:6:9: N806 Variable `GLOBAL_VAR` in function should be lowercase
```


---

_Label `bug` added by @charliermarsh on 2022-11-17 21:36_

---

_Comment by @charliermarsh on 2022-11-17 21:38_

Good catch, will fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-17 21:44_

---

_Closed by @charliermarsh on 2022-11-17 22:19_

---

_Comment by @Borda on 2023-01-27 13:10_

@charliermarsh I see the same issue with `v0.0.233`
adding it in: https://github.com/Lightning-AI/metrics/pull/1464
```py
MAX_PORT = 8100
START_PORT = 8088
CURRENT_PORT = START_PORT


def setup_comm(rank, world_size):
    global CURRENT_PORT

    CURRENT_PORT += 1
    if CURRENT_PORT > MAX_PORT:
        CURRENT_PORT = START_PORT  # <- N806 Variable `CURRENT_PORT` in function should be lowercase
```

---
