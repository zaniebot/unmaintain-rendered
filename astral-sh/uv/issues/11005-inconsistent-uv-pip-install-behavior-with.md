---
number: 11005
title: "Inconsistent `uv pip install` behavior with workspace member that shadows pypi package"
type: issue
state: open
author: konstin
labels:
  - bug
assignees: []
created_at: 2025-01-27T21:33:44Z
updated_at: 2025-01-27T21:33:44Z
url: https://github.com/astral-sh/uv/issues/11005
synced_at: 2026-01-10T01:25:00Z
---

# Inconsistent `uv pip install` behavior with workspace member that shadows pypi package

---

_Issue opened by @konstin on 2025-01-27 21:33_

### Summary

For this scenario there's a root package and a workspace member that shadows the root package, here `anyio 0.1.0` locally shadowing `anyio 4.8.0` on pypi. In real-world cases, such divergence can happen when a workspace member is published to pypi and has e.g. a higher version locally that is not yet published to pypi.

Currently, `uv pip install` will accept `anyio>=4` depending on whether it exists in the venv, ignoring a conflict with the anyio 0.1.0 of the workspace member that the workspace root depends on. Or put the other way round, `uv pip install` won't find an `anyio>=4` if it's shadowed by a local workspace member, even though it exists on pypi.

It's not fully clear to me what the right behavior in this scenario is, but the behavior is unexpected.

```
uv init --app foo
cd foo
uv init --app anyio
uv add ./anyio
uv-debug pip install --no-sources --upgrade .
uv-debug pip install . "anyio>=4" && echo "success" || echo "failure"
uv-debug pip install --upgrade .
uv-debug pip install . "anyio>=4" && echo "success" || echo "failure"
```

### Platform

Ubuntu 24.04 x86_64

### Version

uv 0.5.24

### Python version

3.13.0

---

_Label `bug` added by @konstin on 2025-01-27 21:33_

---
