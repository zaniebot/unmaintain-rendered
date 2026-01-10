```yaml
number: 3096
title: "`F821: Undefined name` in `match` capture patterns"
type: issue
state: closed
author: mikeroll
labels:
  - bug
assignees: []
created_at: 2023-02-21T21:29:11Z
updated_at: 2023-02-21T22:04:11Z
url: https://github.com/astral-sh/ruff/issues/3096
synced_at: 2026-01-10T11:09:46Z
```

# `F821: Undefined name` in `match` capture patterns

---

_Issue opened by @mikeroll on 2023-02-21 21:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
First of all, big thanks and congrats on shipping `match` support! Looks like it's not quite perfect yet as I catch a false-positive F821 with a very simple snippet:
```python
def test(provided: int) -> int:
    match provided:
        case captured:
            return captured  # F821 Undefined name `captured`
```
```sh
❯ ruff --version
ruff 0.0.250
```



---

_Label `bug` added by @charliermarsh on 2023-02-21 21:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-21 21:29_

---

_Comment by @charliermarsh on 2023-02-21 21:29_

Thank you!

---

_Comment by @charliermarsh on 2023-02-21 21:31_

Will fix this. Please keep filing issues you run into with `match`. My focus was primarily on getting the syntax working (and traversals working within the linter), so the rules probably have rough edges.

---

_Comment by @dpassen on 2023-02-21 21:42_

Not sure if this is the same issue or just very similar:

```python
from dataclasses import dataclass


@dataclass
class Car:
    make: str
    model: str


match Car("Toyota", "Corolla"):
    case Car("Toyota", model):
        print(f"Nice {model}. I love what you do for me, Toyota!")

```

also yields an F821 Undefined name 'model'

---

_Comment by @charliermarsh on 2023-02-21 21:45_

Same issue!

---

_Closed by @charliermarsh on 2023-02-21 22:04_

---
