---
number: 2925
title: Ruff mistakes with Protocol and positional-only arguments.
type: issue
state: closed
author: nstarman
labels:
  - bug
assignees: []
created_at: 2023-02-15T15:06:41Z
updated_at: 2023-02-15T15:25:19Z
url: https://github.com/astral-sh/ruff/issues/2925
synced_at: 2026-01-07T13:12:14-06:00
---

# Ruff mistakes with Protocol and positional-only arguments.

---

_Issue opened by @nstarman on 2023-02-15 15:06_

I'm not sure if I've dug down to the root of the issue, but the following code incorrectly produces the following errors. When I remove the `/` and make `array` positional-or-keyword, rather than positional-only, the errors disappear.

```python
Array = TypeVar("Array")

class SomeCallable(Protocol):
    def __call__(self, array: Array, /, key: Any) -> Array:
        ...
```

```
[tool.ruff]
target-version = "py310"
select = ["ALL"]
```

```
>>> pre-commit run ruff -a

.../data.py:258:18: ANN001 Missing type annotation for function argument `self`
.../data.py:258:41: N805 First argument of a method should be named `self`
```

- [x] A minimal code snippet that reproduces the bug.
- [x] The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- [x] The current Ruff settings (any relevant sections from your `pyproject.toml`).
- [x] The current Ruff version is "v0.0.243"


---

_Comment by @charliermarsh on 2023-02-15 15:12_

Thank you!

---

_Label `bug` added by @charliermarsh on 2023-02-15 15:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-15 15:14_

---

_Comment by @charliermarsh on 2023-02-15 15:16_

I think the `N805` error is fixed on `main` already.

---

_Referenced in [astral-sh/ruff#2927](../../astral-sh/ruff/pulls/2927.md) on 2023-02-15 15:19_

---

_Closed by @charliermarsh on 2023-02-15 15:25_

---
