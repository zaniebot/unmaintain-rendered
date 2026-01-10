---
number: 9548
title: "Resolving `toga-gtk` attempts to build `pygobject` on MacOS"
type: issue
state: closed
author: ncoghlan
labels:
  - enhancement
assignees: []
created_at: 2024-12-01T08:16:58Z
updated_at: 2024-12-04T11:40:32Z
url: https://github.com/astral-sh/uv/issues/9548
synced_at: 2026-01-10T01:24:42Z
---

# Resolving `toga-gtk` attempts to build `pygobject` on MacOS

---

_Issue opened by @ncoghlan on 2024-12-01 08:16_

`toga-gtk` fails to generate a valid cross-platform lock file when added as a dependency on platforms other than Linux, as `pygobject` can only be published as an sdist: https://github.com/beeware/toga/issues/2995

(statically linking the GObject bindings is not desirable, and a valid manylinux wheel can't just assume those libraries exist)

Setting `tool.uv.dependency.metadata` doesn't solve the problem, since that's only effective for application locks - the tool table metadata doesn't make it into the `toga-gtk` `.dist-info` folder,  so `uv add` doesn't have access to it when the dependency on `toga-gtk` is resolved.

Searching the code for `build_wheel` and `prepare_metadata_for_build_wheel`, it isn't clear to me if the problem is that `uv` simply isn't trying the latter option before progressing to the full wheel build, or if the latter isn't working with meson-build, so `uv` is falling back to the former, which than fails due to the platform incompatibility.

Potentially related:

* architecture mismatch issue: https://github.com/astral-sh/uv/issues/3308 
* meson-build issue: https://github.com/astral-sh/uv/issues/8966


---

_Renamed from "`uv` attempts to build `pygobject` on MacOS when resolving `toga-gtk` dependencies" to "`uv` attempts to build `pygobject` on MacOS when resolving `toga-gtk`" by @ncoghlan on 2024-12-01 08:17_

---

_Renamed from "`uv` attempts to build `pygobject` on MacOS when resolving `toga-gtk`" to "Resolving `toga-gtk` attempts to build `pygobject` on MacOS" by @ncoghlan on 2024-12-01 08:17_

---

_Referenced in [beeware/toga#2995](../../beeware/toga/issues/2995.md) on 2024-12-01 08:21_

---

_Referenced in [astral-sh/uv#9549](../../astral-sh/uv/pulls/9549.md) on 2024-12-01 09:05_

---

_Label `enhancement` added by @konstin on 2024-12-04 11:27_

---

_Closed by @konstin on 2024-12-04 11:40_

---

_Closed by @konstin on 2024-12-04 11:40_

---
