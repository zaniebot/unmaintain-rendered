---
number: 13829
title: "`[tool.uv.sources]` should warn if it has no effect on any platform"
type: issue
state: open
author: oconnor663
labels:
  - enhancement
assignees: []
created_at: 2025-06-04T03:14:03Z
updated_at: 2025-06-04T19:42:37Z
url: https://github.com/astral-sh/uv/issues/13829
synced_at: 2026-01-10T01:25:39Z
---

# `[tool.uv.sources]` should warn if it has no effect on any platform

---

_Issue opened by @oconnor663 on 2025-06-04 03:14_

### Summary

This came up in https://github.com/astral-sh/uv/issues/13774. Consider a `pyproject.toml` file shaped like this:

```
[project]
name = "debug"
version = "0.1.0"
dependencies = [
  "pandas",
]

[tool.uv.sources]
numpy = { url = "https://files.pythonhosted.org/packages/19/49/4df9123aafa7b539317bf6d342cb6d227e49f7a35b99c287a6109b13dd93/numpy-2.2.6-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" }
```

The intent might be to influence what version of `numpy` gets used as a dependency for `pandas`, but because `numpy` doesn't appear in the `dependencies` list, the `sources` config has no effect at all. Here's a more confusing version of the same problem:

```
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "pandas",
  "numpy; python_version < '3.13'",
]

[tool.uv.sources]
numpy = { 
    url = "https://files.pythonhosted.org/packages/19/49/4df9123aafa7b539317bf6d342cb6d227e49f7a35b99c287a6109b13dd93/numpy-2.2.6-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl",
    marker = "python_version >= '3.13'" 
}
```

Here `numpy` does appear in the `dependencies` list, but the markers are _disjoint_, and it's still impossible for the `sources` config to affect any target. Ideally we should warn in both cases. The latter might require getting involved in `lowering.rs`?

### Platform

platform-independent

### Version

0.7.9

### Python version

_No response_

---

_Assigned to @oconnor663 by @oconnor663 on 2025-06-04 03:14_

---

_Label `enhancement` added by @oconnor663 on 2025-06-04 03:14_

---

_Referenced in [astral-sh/uv#13774](../../astral-sh/uv/issues/13774.md) on 2025-06-04 03:14_

---

_Comment by @charliermarsh on 2025-06-04 19:42_

Important to note that we'd need to check that `numpy` isn't a dependency in any workspace member (not just the root `pyproject.toml`), since sources apply to workspace members too.

---

_Referenced in [astral-sh/uv#13900](../../astral-sh/uv/issues/13900.md) on 2025-06-08 18:04_

---

_Referenced in [astral-sh/uv#13981](../../astral-sh/uv/issues/13981.md) on 2025-06-12 00:45_

---

_Referenced in [astral-sh/uv#14077](../../astral-sh/uv/pulls/14077.md) on 2025-06-16 14:59_

---
