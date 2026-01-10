```yaml
number: 16445
title: tweak build dependency hint wording
type: pull_request
state: open
author: KTibow
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-10-25T01:23:04Z
updated_at: 2025-10-29T14:52:00Z
url: https://github.com/astral-sh/uv/pull/16445
synced_at: 2026-01-10T06:36:16Z
```

# tweak build dependency hint wording

---

_Pull request opened by @KTibow on 2025-10-25 01:23_

## Summary
old hint wording is like
```
hint: This error likely indicates that `pufferlib@3.0.0` depends on `pybind11`, but doesn't declare it as a build
dependency. If `pufferlib` is a first-party package, consider adding `pybind11` to its `build-system.requires`.
Otherwise, either add it to your `pyproject.toml` under:

[tool.uv.extra-build-dependencies]
pufferlib = ["pybind11"]

or `uv pip install pybind11` into the environment and re-run with `--no-build-isolation`.
```
which, in the case where it's a first-party package, is a little vague and deemphasized.

this pr uses wording that treats both cases as equals:
```
hint: This error likely indicates that `pufferlib@3.0.0` depends on `pybind11`, but doesn't declare it as a build
dependency. You likely should tweak your `pyproject.toml`:
# if pufferlib is first-party
[build-system]
requires = ["pybind11"]
# otherwise
[tool.uv.extra-build-dependencies]
pufferlib = ["pybind11"]

or `uv pip install pybind11` into the environment and re-run with `--no-build-isolation`.
```

---

_Converted to draft by @KTibow on 2025-10-25 01:32_

---

_Marked ready for review by @KTibow on 2025-10-25 02:24_

---

_Comment by @zanieb on 2025-10-28 12:23_

I think we need to consider how we want to do multiline code formatting in hints like this. I believe that's why we have different instructions for the first-party case (it's also less common, and I think it's probably fair to de-emphasize?)

---

_Comment by @KTibow on 2025-10-28 21:06_

If this change doesn't make sense, an alternative is to automatically detect whether the package seems first-party (not necessarily with rigor). Then this deemphasis would be very logical.

---

_Comment by @zanieb on 2025-10-29 14:52_

First-party in this case could just be that you _own_ the package. There's no way easy to detect that.

---
