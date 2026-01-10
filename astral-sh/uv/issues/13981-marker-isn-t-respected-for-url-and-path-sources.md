---
number: 13981
title: "`marker` isn't respected for URL and path sources"
type: issue
state: closed
author: oconnor663
labels:
  - bug
assignees: []
created_at: 2025-06-12T00:45:55Z
updated_at: 2025-06-12T17:45:34Z
url: https://github.com/astral-sh/uv/issues/13981
synced_at: 2026-01-10T01:25:41Z
---

# `marker` isn't respected for URL and path sources

---

_Issue opened by @oconnor663 on 2025-06-12 00:45_

Here's an example:
```
[project]
name = "scratch"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "numpy",
]

[tool.uv.sources.numpy]
git = "https://github.com/numpy/numpy"
```
This gives me:
```
$ (uv sync --upgrade && uv pip show numpy) 2>&1 | grep Version
Version: 2.4.0.dev0
```
Now if I add a marker that disables that source (I'm on Linux, not Windows), the resolved version changes back to the latest published version, as expected:
```
[tool.uv.sources.numpy]
git = "https://github.com/numpy/numpy"
marker = "sys_platform == 'win32'"
```
```
$ (uv sync --upgrade && uv pip show numpy) 2>&1 | grep Version
Version: 2.3.0
```
However, here's the part I didn't expect. Using a URL source or a path source behaves differently. A URL source affects the version even when it should be disabled by a marker:
```
[tool.uv.sources.numpy]
url = "https://files.pythonhosted.org/packages/19/49/4df9123aafa7b539317bf6d342cb6d227e49f7a35b99c287a6109b13dd93/numpy-2.2.6-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl"
marker = "sys_platform == 'win32'"
```
```
$ (uv sync --upgrade && uv pip show numpy) 2>&1 | grep Version
Version: 2.2.6
```
This seems like a bug. (Found while investigating https://github.com/astral-sh/uv/issues/13829.)

---

_Label `bug` added by @oconnor663 on 2025-06-12 00:45_

---

_Assigned to @oconnor663 by @oconnor663 on 2025-06-12 00:46_

---

_Comment by @oconnor663 on 2025-06-12 16:37_

Additional info: If I set a version constraint that doesn't like the version from `sources`, then URL behaves like Git as expected.

```
dependencies = [
    "numpy>=2.3",
]
```

But if the constraint allows the version from `sources`, I get the same inconsistent behavior as with no constraint.

```
dependencies = [
    "numpy>=2.2",
]
```

---

_Comment by @oconnor663 on 2025-06-12 17:45_

Never mind. The difference here seems to be that `2.4.0.dev0` is a prerelease version and `2.2.6` is not. Git and Url sources behave the same when they're both given the same version.

---

_Closed by @oconnor663 on 2025-06-12 17:45_

---
