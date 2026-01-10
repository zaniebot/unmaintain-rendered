---
number: 16496
title: Build resolutions need different hints
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2025-10-29T14:48:00Z
updated_at: 2025-11-02T18:38:10Z
url: https://github.com/astral-sh/uv/issues/16496
synced_at: 2026-01-10T01:26:06Z
---

# Build resolutions need different hints

---

_Issue opened by @zanieb on 2025-10-29 14:48_

When a build resolution fails, we don't go through our `NoSolutionError` hint generation. e.g., in a case like https://github.com/astral-sh/uv/issues/16494 we don't provide our pre-release hint.

This doesn't look trivial.

cc @konstin as you may have the best context on how to do this.

---

_Label `error messages` added by @zanieb on 2025-10-29 14:48_

---

_Renamed from "Build resolution errors should include hints" to "Build resolutions have fixed prerelease flexibility" by @konstin on 2025-10-29 15:17_

---

_Renamed from "Build resolutions have fixed prerelease flexibility" to "Build resolutions need different hints" by @konstin on 2025-10-29 15:19_

---

_Comment by @konstin on 2025-10-29 15:21_

We are generating hints for build resolutions, but but the build resolutions work different wrt to prereleases, their options are fixed:

https://github.com/astral-sh/uv/blob/accfb488766f02c7c975f32243c82f1eed14b68f/crates/uv-dispatch/src/lib.rs#L256

This option exists to not recommend setting `--prerelease=allow`, which source dist builds ignore (https://github.com/astral-sh/uv/pull/8192). We need separate hints for source dist builds.

---

_Comment by @konstin on 2025-10-29 15:23_

MRE:

```toml
[project]
name = "test"
version = "0.0.0"
requires-python = ">=3.13"

[build-system]
requires = ["transitive-package-only-prereleases-in-range-a"]
build-backend = "setuptools.build_meta"
```

```
uv sync --index-url https://astral-sh.github.io/packse/0.3.53/simple-html/ --prerelease=allow
```

---

_Comment by @zanieb on 2025-10-31 03:55_

Oh, thanks for the correction!

Alas then my point at https://github.com/astral-sh/uv/issues/16494#issuecomment-3461805250 is incorrect.

What should we be recommending instead?

---

_Referenced in [astral-sh/uv#16494](../../astral-sh/uv/issues/16494.md) on 2025-10-31 03:55_

---

_Comment by @konstin on 2025-10-31 09:35_

For now I'd just say that indirect prereleases are not supported in build system requires. We do have `uv build --build-constraint` if you pass e.g. `transitive-package-only-prereleases-in-range-b>=0a1`, but that's not supported in `uv sync`.

---

_Referenced in [astral-sh/uv#16550](../../astral-sh/uv/pulls/16550.md) on 2025-11-02 04:22_

---

_Closed by @charliermarsh on 2025-11-02 18:38_

---
