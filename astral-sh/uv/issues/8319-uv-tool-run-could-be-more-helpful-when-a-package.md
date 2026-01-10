---
number: 8319
title: "`uv tool run` could be more helpful when a package name and tool don't match"
type: issue
state: closed
author: akx
labels:
  - good first issue
  - help wanted
  - error messages
assignees: []
created_at: 2024-10-18T06:31:54Z
updated_at: 2024-10-22T19:16:11Z
url: https://github.com/astral-sh/uv/issues/8319
synced_at: 2026-01-10T01:24:27Z
---

# `uv tool run` could be more helpful when a package name and tool don't match

---

_Issue opened by @akx on 2024-10-18 06:31_

macOS arm64, uv 0.4.24

```
$ uv tool run cyclonedx-bom
The executable `cyclonedx-bom` was not found.
warning: An executable named `cyclonedx-bom` is not provided by package `cyclonedx-bom`.
The following executables are provided by `cyclonedx-bom`:
- cyclonedx-py
```

Suggestions:

* This help text could tell me what to do to make this work (the magic spell `uv tool run --from cyclonedx-bom cyclonedx-py`).
  * `uv tool run cyclonedx-py --from cyclonedx-bom` already has this sort of warning in place: `warning: An executable named `cyclonedx-py` is not provided by package `cyclonedx-py` but is available via the dependency `cyclonedx-bom`. Consider using `uv tool run --from cyclonedx-bom cyclonedx-py` instead.`

* Alternatively, this could Do What I Mean and at least prompt me if I want to run the similarly-named single option, if not run it directly?

---

_Comment by @charliermarsh on 2024-10-18 17:59_

I like adding the `uv tool run --from cyclonedx-bom cyclonedx-py` hint at the end.

---

_Label `help wanted` added by @charliermarsh on 2024-10-18 17:59_

---

_Label `error messages` added by @charliermarsh on 2024-10-18 17:59_

---

_Label `good first issue` added by @charliermarsh on 2024-10-18 17:59_

---

_Referenced in [astral-sh/uv#8473](../../astral-sh/uv/pulls/8473.md) on 2024-10-22 18:17_

---

_Closed by @zanieb on 2024-10-22 19:16_

---

_Closed by @zanieb on 2024-10-22 19:16_

---
