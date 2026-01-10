```yaml
number: 12578
title: "SIM118: Error on any method called `.keys()`, not just dictionaries"
type: issue
state: closed
author: ocaballeror
labels:
  - type-inference
assignees: []
created_at: 2024-07-30T12:47:30Z
updated_at: 2025-06-13T12:55:05Z
url: https://github.com/astral-sh/ruff/issues/12578
synced_at: 2026-01-10T11:09:54Z
```

# SIM118: Error on any method called `.keys()`, not just dictionaries

---

_Issue opened by @ocaballeror on 2024-07-30 12:47_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.

"SIM118" and "keys"
* A minimal code snippet that reproduces the bug.
```
class T:
    def keys(self) -> list[int]:
        return [1, 2, 3]


for key in T().keys():  # SIM118
    print(key)
```
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
```
ruff check --isolated --no-cache --select SIM118 asdf.py
```
* The current Ruff settings (any relevant sections from your `pyproject.toml`).

None
* The current Ruff version (`ruff --version`).
```
ruff 0.8.1
```

---

_Label `type-inference` added by @charliermarsh on 2024-07-30 16:36_

---

_Comment by @LordAro on 2025-06-13 09:25_

This seems to be a duplicate of #4481

---

_Comment by @ntBre on 2025-06-13 12:55_

Thanks, I think you're right!

---

_Closed by @ntBre on 2025-06-13 12:55_

---
