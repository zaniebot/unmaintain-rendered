---
number: 16144
title: "`uv add` silently ignores version constraint that can't be resolved"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2025-10-07T00:35:58Z
updated_at: 2025-10-07T01:03:23Z
url: https://github.com/astral-sh/uv/issues/16144
synced_at: 2026-01-10T01:26:03Z
---

# `uv add` silently ignores version constraint that can't be resolved

---

_Issue opened by @DetachHead on 2025-10-07 00:35_

### Summary

```toml
# pyproject.toml
[project]
name = "asdf"
version = "0.0.1"
dependencies = []
requires-python = ">=3.8,<4.0"
readme = "README.md"
license = { text = "MIT" }
```

when running `uv add "selectolax>=0.4.0"` the version constraint is silently ignored and `"selectolax>=0.3.29"` is added instead, i think because that's the last version that supports python 3.8

### Platform

windows 11

### Version

0.28.3

### Python version

3.13

---

_Label `bug` added by @DetachHead on 2025-10-07 00:35_

---

_Comment by @charliermarsh on 2025-10-07 00:49_

I can't reproduce this:
```
❯ uv add "selectolax>=0.4.0"
  × No solution found when resolving dependencies for split (markers: python_full_version == '3.8.*'):
  ╰─▶ Because the requested Python version (>=3.8, <4.0) does not satisfy Python>=3.9 and selectolax==0.4.0 depends on Python>=3.9, we can conclude that selectolax==0.4.0 cannot be used.
      And because only selectolax<=0.4.0 is available and your project depends on selectolax>=0.4.0, we can conclude that your project's requirements are unsatisfiable.

      hint: The `requires-python` value (>=3.8, <4.0) includes Python versions that are not supported by your dependencies (e.g., selectolax==0.4.0 only supports >=3.9). Consider using a more restrictive `requires-python` value (like
      >=3.9).

      hint: While the active Python version is 3.13, the resolution failed for other Python versions supported by your project. Consider limiting your project's supported Python versions using `requires-python`.
```

---

_Comment by @DetachHead on 2025-10-07 01:03_

hmm, seems like this is a pyprojectx issue actually. it only happens when running uv through the `./pw` wrapper script, eg:

```
./pw uv add "selectolax>=0.4.0"
```

apologies for the noise

---

_Closed by @DetachHead on 2025-10-07 01:03_

---

_Referenced in [pyprojectx/pyprojectx#159](../../pyprojectx/pyprojectx/issues/159.md) on 2025-10-07 01:18_

---
