---
number: 12691
title: "`uv run --with` uses different dependencies when prefixed with `python`"
type: issue
state: open
author: provinzkraut
labels:
  - bug
assignees: []
created_at: 2025-04-06T11:11:46Z
updated_at: 2025-05-27T07:56:27Z
url: https://github.com/astral-sh/uv/issues/12691
synced_at: 2026-01-10T01:25:23Z
---

# `uv run --with` uses different dependencies when prefixed with `python`

---

_Issue opened by @provinzkraut on 2025-04-06 11:11_

### Summary

```toml
[project]
name = "uv-stuff"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "typing-extensions==4.13.1",
    "pytest"
]
```

```python
from importlib.metadata import version

def test():
    assert version("typing_extensions") == "4.13.0"
```

**This works**
`uv run --with=typing-extensions==4.13.0 python -m pytest test.py`

**This fails**
`uv run --with=typing-extensions==4.13.0 pytest test`
Because the version specified in the `pyproject.toml` (`4.13.1`) is used, i.e. the version specified in `--with` is ignored.

I would have expected this to work in both cases, as the documentation states that
> `uv run file.py` is equivalent to `uv run python file.py`

### Platform

Ubuntu 24.10 x86_64

### Version

0.6.12

### Python version

3.13.0

---

_Label `bug` added by @provinzkraut on 2025-04-06 11:11_

---

_Comment by @charliermarsh on 2025-04-06 21:04_

I think this is expected since invoking `pytest` like that will use the executable in your existing environment.

---

_Comment by @provinzkraut on 2025-04-07 07:53_

Mmh. I was under the impression that uv would ensure that the command I run with `run` is executed in the environment managed by uv, and that this would apply to ephemeral environments, like the one created by e.g. `--with`, as well.

But if `uv run --with=xyz pytest` uses the existing environment, why doesn't `uv run --with=xyz python`? I would have expected them to both just be links into the same `.venv/bin/`

---

_Comment by @charliermarsh on 2025-04-07 13:16_

It has to do with the way environment layering is implemented (which you shouldn't need to care about, but it does explain the behavior). When we use `--with`, we create a new environment and give it access to the existing environment's `site-packages`. So `uv run --with=xyz python` uses the Python from the "new" environment, but `uv run --with=xyz pytest` uses the `pytest` from the "old" environment.

---

_Comment by @provinzkraut on 2025-04-07 14:04_

Okay, that makes sense. It's still a bit counter intuitive to me, especially after reading the docs. Maybe a section about this difference in behaviour would be useful? 

---

_Comment by @dedebenui on 2025-05-27 07:56_

That makes sense, `uv run --with foo pytest` will call the `pytest` script in `.venv/bin`, which has a shebang linking to the Python executable in the virtual environment, which doesn't see `foo` because it is installed in a temporary environment layered on top.

So, is there any situation where `uv run --with foo` makes sense except calling `-m`, `python`, or any script in `foo` ? If not, I think at least a warning should be emitted, explaining that the user probably intended `uv run --with foo -m pytest` or `uv run --with foo python -m pytest`.


---
