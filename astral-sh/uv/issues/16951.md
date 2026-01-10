```yaml
number: 16951
title: uv cannot pick python3.14t
type: issue
state: closed
author: mbUSC
labels:
  - question
assignees: []
created_at: 2025-12-03T03:52:09Z
updated_at: 2025-12-03T05:52:06Z
url: https://github.com/astral-sh/uv/issues/16951
synced_at: 2026-01-10T03:23:55Z
```

# uv cannot pick python3.14t

---

_Issue opened by @mbUSC on 2025-12-03 03:52_

### Summary

Functionality is broken -- I am not able to select `3.14t`:

```
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 4, column 19
  |
4 | requires-python = ">=3.14t"
  |                   ^^^^^^^^^
Failed to parse version: after parsing `3.14`, found `t`, which is not part of a valid version:
>=3.14t
^^^^^^^
```

### Platform

macOS 26

### Version

uv 0.9.9 (4fac4cb7e 2025-11-12)

### Python version

3.14t

---

_Label `bug` added by @mbUSC on 2025-12-03 03:52_

---

_Comment by @charliermarsh on 2025-12-03 04:04_

That's not a valid value for `requires-python`. The valid values for `requires-python` are governed by a standard -- it has to be a valid version specifier.

You should instead put `3.14t` in a `.python-version` file in the same directory.

`requires-python` declares the range of Python versions compatible with your project; `.python-version` tells tools which Python version to select.

---

_Label `bug` removed by @charliermarsh on 2025-12-03 04:04_

---

_Label `question` added by @charliermarsh on 2025-12-03 04:04_

---

_Closed by @charliermarsh on 2025-12-03 05:52_

---
