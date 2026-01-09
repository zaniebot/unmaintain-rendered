---
number: 10644
title: "Feature request: Allow overriding `requires-python` value for all dependencies and sub-dependencies during install/sync"
type: issue
state: open
author: keller-mark
labels: []
assignees: []
created_at: 2025-01-15T20:56:09Z
updated_at: 2025-01-15T20:56:09Z
url: https://github.com/astral-sh/uv/issues/10644
synced_at: 2026-01-07T13:12:18-06:00
---

# Feature request: Allow overriding `requires-python` value for all dependencies and sub-dependencies during install/sync

---

_Issue opened by @keller-mark on 2025-01-15 20:56_

Use case: I finally get a working environment with many dependencies and Python v3.9. Then, I need to install some new package X which has a `requires-python = ">=3.10"` and some of its sub-dependencies such as Y also have  `requires-python = ">=3.10"`, despite still working fine with v3.9. I need to create a whole new environment with a different python v3.10 version now, just to add this dependency.

While it may be the case that the packages X and Y actually do _require_ v3.10, I have noticed that in other cases package authors seem to use `requires-python` to reduce testing surface area or encourage downstream users to avoid end-of-life versions, rather than strictly requiring a particular python version.

I tried 

```toml
[[tool.uv.dependency-metadata]]
name = "X"
version = "1.0.0"
requires-python = ">=3.9"

[[tool.uv.dependency-metadata]]
name = "Y"
version = "1.0.0"
requires-python = ">=3.9"
```

but this fails as only the X metadata is considered (not Y metadata as it is an indirect dependency via X).

A different approach would be to allow for an environment variable such as `UV_OVERRIDE_REQUIRES_PYTHON=">=3.9"` if I believe that `requires-python` for dependencies and subdependencies are too strict.



---
