```yaml
number: 17229
title: Use ty to type-check Python files
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: ty
created_at: 2025-12-23T13:22:05Z
updated_at: 2026-01-07T18:25:15Z
url: https://github.com/astral-sh/uv/pull/17229
synced_at: 2026-01-10T05:49:14Z
```

# Use ty to type-check Python files

---

_Pull request opened by @j178 on 2025-12-23 13:22_

I added mypy type‑checking in https://github.com/astral-sh/uv/pull/5332, so I think it's a good time to switch to ty now :)

---

_Label `internal` added by @konstin on 2026-01-06 19:22_

---

_Review requested from @zanieb by @konstin on 2026-01-06 19:23_

---

_@zanieb reviewed on 2026-01-06 19:32_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:97 on 2026-01-06 19:32_

Is this a known false positive?

---

_@zanieb approved on 2026-01-06 19:32_

Thanks!

---

_@zanieb reviewed on 2026-01-06 19:36_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:97 on 2026-01-06 19:36_

Alex confirmed this is missing from typeshed and is not a ty bug, however, this attribute does exist on my machine.

---

_@woodruffw reviewed on 2026-01-07 18:21_

---

_Review comment by @woodruffw on `python/uv/_find_uv.py`:97 on 2026-01-07 18:21_

Relevant CPython source:

https://github.com/python/cpython/blob/9a3263ff8f87114285fe7c4cf61c075fd67a2ca3/Python/sysmodule.c#L3871

Where `_PYTHONFRAMEWORK` in turn is an empty string by default, or the framework name from the build system (presumably for macOS only).

---

_Merged by @zanieb on 2026-01-07 18:25_

---

_Closed by @zanieb on 2026-01-07 18:25_

---
