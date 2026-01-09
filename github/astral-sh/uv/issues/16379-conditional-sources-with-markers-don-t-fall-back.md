---
number: 16379
title: "Conditional sources with markers don't fall back to registry"
type: issue
state: closed
author: ameicler
labels:
  - bug
assignees: []
created_at: 2025-10-21T06:41:29Z
updated_at: 2025-10-21T09:03:39Z
url: https://github.com/astral-sh/uv/issues/16379
synced_at: 2026-01-07T13:12:19-06:00
---

# Conditional sources with markers don't fall back to registry

---

_Issue opened by @ameicler on 2025-10-21 06:41_

### Summary

When using a source override with a marker like:

``` toml
[tool.uv.sources]
habitat-sim = { path = "...", marker = "extra == 'cuda'" } # e.g, `path. = "cuda/habitat_sim-0.3.3-cp310-cp310-linux_x86_64.whl"`
```

Expected: When marker doesn't match, fall back to registry
Actual: uv lock only captures the path source, ignores marker, and fails to install when marker is false

This prevents environment-specific wheel usage (CUDA vs CPU builds).

### Platform

Linux 5.10.242-239.961.amzn2.x86_64 x86_64 GNU/Linux

### Version

uv 0.9.4

### Python version

Python 3.10.19

---

_Label `bug` added by @ameicler on 2025-10-21 06:41_

---

_Comment by @konstin on 2025-10-21 08:14_

That looks like the same bug as https://github.com/astral-sh/uv/issues/14090. Does `extra = 'cuda'` help?

---

_Comment by @ameicler on 2025-10-21 08:45_

Yes, it seems like the same bug indeed. With`extra="cuda"`, when I run `uv sync` it does not install the library (registry fallback) at all which is what one would expect.

---

_Comment by @konstin on 2025-10-21 08:53_

Let's track this in https://github.com/astral-sh/uv/issues/14090

---

_Closed by @konstin on 2025-10-21 08:53_

---

_Referenced in [astral-sh/uv#14090](../../astral-sh/uv/issues/14090.md) on 2025-10-21 09:02_

---
