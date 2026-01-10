```yaml
number: 9479
title: "SIM222: false positive on format strings"
type: issue
state: closed
author: reitzig
labels:
  - bug
assignees: []
created_at: 2024-01-11T23:38:55Z
updated_at: 2024-01-12T02:16:21Z
url: https://github.com/astral-sh/ruff/issues/9479
synced_at: 2026-01-10T11:09:51Z
```

# SIM222: false positive on format strings

---

_Issue opened by @reitzig on 2024-01-11 23:38_

Consider this script:

`repro.py`
```python
def foo(a: str, b: str) -> None:
    print(f"{a}{b}" or "bar")


if __name__ == "__main__":
    foo("Hello ", "World")
    foo("", "")
```

When run, it prints:

```
❯ python repro.py
Hello World
bar
```

So apparently, `f"{a}{b}"` is not a truthy constant! And yet:

```shell
❯ ruff --version
ruff 0.1.11
❯ ruff check --isolated --select=SIM repro.py
repro.py:2:11: SIM222 Use `f"{a}{b}"` instead of `f"{a}{b}" or ...`
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

FWIW, SonarLint's python:S5797 triggers as well.

---

_Comment by @charliermarsh on 2024-01-12 01:21_

Thanks, I actually thought we had handling for this, but apparently not :)

---

_Label `bug` added by @charliermarsh on 2024-01-12 01:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-12 01:27_

---

_Closed by @charliermarsh on 2024-01-12 02:16_

---
