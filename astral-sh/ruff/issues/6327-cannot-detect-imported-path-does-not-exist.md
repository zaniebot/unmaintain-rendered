```yaml
number: 6327
title: Cannot detect imported path does not exist
type: issue
state: open
author: redcray
labels:
  - rule
assignees: []
created_at: 2023-08-04T03:32:33Z
updated_at: 2024-07-26T02:02:05Z
url: https://github.com/astral-sh/ruff/issues/6327
synced_at: 2026-01-12T15:54:46Z
```

# Cannot detect imported path does not exist

---

_@redcray_

xx.aa is a non-existent path, ruff indeed cannot resolve to this path, but did not give any hint.
I don't know the specific rules of grammar detection, but I think this is a feature that can be supported?

Thanks!

* A minimal code snippet that reproduces the bug.

```
from xx.aa import Config
Config()
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```
ruff check .
```

* The current Ruff settings (any relevant sections from your `pyproject.toml`).
none

* The current Ruff version (`ruff --version`).
ruff 0.0.282



---

_Label `rule` added by @charliermarsh on 2023-08-16 04:03_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-16 04:05_

---

_Comment by @charliermarsh on 2023-08-16 04:05_

We wrote a Python module resolver that can support this in theory, but the details are hard to get right, since we might need to know about your virtualenv to know if the module is third-party.

---

_Comment by @charliermarsh on 2023-08-16 04:06_

On second thought, we could just detect whether this package (`xx`) is first-party, and then see if the module exists within the first-party package, and raise if not.

---

_Label `needs-decision` removed by @charliermarsh on 2024-07-26 02:02_

---
