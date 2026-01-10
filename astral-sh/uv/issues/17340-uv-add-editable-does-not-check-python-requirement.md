---
number: 17340
title: "`uv add --editable` does not check python requirement"
type: issue
state: closed
author: rzuckerm
labels:
  - question
assignees: []
created_at: 2026-01-06T22:00:37Z
updated_at: 2026-01-06T22:32:27Z
url: https://github.com/astral-sh/uv/issues/17340
synced_at: 2026-01-10T01:26:16Z
---

# `uv add --editable` does not check python requirement

---

_Issue opened by @rzuckerm on 2026-01-06 22:00_

### Summary

- Create a new package called `test-pkg`:
  ```
  uv new --package test-pkg
  cd test-pkg
  ```
- In `test-pkg/pyproject.toml` change `requires-python` to `>=3.11,<4`
- Run `uv lock`
- Create a new package call `test-pkg-b`
  ```
  cd ..
  uv new --package test-pkg-b
  cd test-pkg-b
  ```
- In `test-pkg-b/pyproject.toml` change `requires-python` to `>=3.10,<3.11`
- Run `uv lock`
- In `test-pkg`, add `test-pkg-b` as an editable dependency:
  ```
  cd ../test-pkg
  uv add --editable ../test-pkg-b
  ```

I would expected the `uv add` command to fail since `test-pkg-b` requires python 3.10 only, and `test-pkg` requires python 3.11 or better. Instead `uv add` runs without error.

### Platform

Ubuntu 24.04 amd64

### Version

0.9.22

### Python version

Python 3.12.3

---

_Label `bug` added by @rzuckerm on 2026-01-06 22:00_

---

_Comment by @zanieb on 2026-01-06 22:04_

We ignore upper bounds on `requires-python`, so the package is not considered incompatible

See 

- https://discuss.python.org/t/requires-python-upper-limits/12663
- https://docs.astral.sh/uv/pip/compatibility/#requires-python-upper-bounds
- https://docs.astral.sh/uv/reference/internals/resolver/#requires-python
- https://github.com/astral-sh/uv/issues/4022



---

_Label `bug` removed by @zanieb on 2026-01-06 22:04_

---

_Label `question` added by @zanieb on 2026-01-06 22:04_

---

_Closed by @rzuckerm on 2026-01-06 22:32_

---
