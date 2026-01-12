```yaml
number: 13297
title: Cannot override dependency metadata of source dependency
type: issue
state: closed
author: toslunar
labels:
  - question
assignees: []
created_at: 2025-05-05T07:19:05Z
updated_at: 2025-05-13T03:05:20Z
url: https://github.com/astral-sh/uv/issues/13297
synced_at: 2026-01-12T16:01:24Z
```

# Cannot override dependency metadata of source dependency

---

_@toslunar_

### Summary

To reproduce, `uv sync` at `a/` with the files below causes
```
  × No solution found when resolving dependencies:
  ╰─▶ Because only b==0.1.0 is available and b==0.1.0 depends on numpy<2, we can conclude that all versions of b
      depend on numpy<2.
      And because your project depends on b and numpy>=2, we can conclude that your project's requirements are
      unsatisfiable.
```

`a/pyproject.toml`
```toml
[project]
name = "a"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["b", "numpy>=2"]

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[tool.uv.sources]
b = { path = "../b", editable = true }

[[tool.uv.dependency-metadata]]
name = "b"
requires-dist = ["numpy"]
```

`a/src/a.py`
```python
...
```

`b/pyproject.toml`
```toml
[project]
name = "b"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["numpy<2"]

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

`b/src/b.py`
```python
...
```

---

Note that for PyPI dependency we can override `requires-dist` (`numpy<1.29.0,>=1.22.4` in `scipy==1.12.0`) by

`c/pyproject.toml`
```toml
[project]
name = "c"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["scipy==1.12.0", "numpy>=2"]

[[tool.uv.dependency-metadata]]
name = "scipy"
requires-dist = ["numpy"]
```

### Platform

Linux 5.15.167.4-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.7.2

### Python version

3.12.9

---

_Label `bug` added by @toslunar on 2025-05-05 07:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-07 22:23_

---

_Comment by @charliermarsh on 2025-05-07 22:27_

I can't remember why it works this way, but it _does_ work if you specify the version:

```toml
[[tool.uv.dependency-metadata]]
name = "b"
version = "0.1.0"
requires-dist = ["numpy"]
```

---

_Comment by @charliermarsh on 2025-05-07 22:28_

Oh, it's because we need to know the version of the package, but for a direct URL dependency like this, we don't know the version in advance -- we have to read it from the package `pyproject.toml`. If we're using static metadata like this, then we need to get _all_ the metadata we need upfront. So if you don't include a version, we can't use the static metadata, since we have to go resolve the version.

---

_Label `bug` removed by @charliermarsh on 2025-05-07 22:28_

---

_Label `question` added by @charliermarsh on 2025-05-07 22:28_

---

_Comment by @charliermarsh on 2025-05-07 22:29_

There's a warning for this if you run with `-v`:

```
WARN No version found in dependency metadata entry for `b`
```

---

_Closed by @charliermarsh on 2025-05-13 03:05_

---

_Reopened by @charliermarsh on 2025-05-13 03:05_

---

_Closed by @charliermarsh on 2025-05-13 03:05_

---
