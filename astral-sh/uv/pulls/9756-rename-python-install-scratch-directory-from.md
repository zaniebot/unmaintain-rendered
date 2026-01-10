```yaml
number: 9756
title: "Rename Python install scratch directory from `.cache` -> `.temp`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/python-install-scratch
created_at: 2024-12-10T00:15:26Z
updated_at: 2024-12-10T19:39:47Z
url: https://github.com/astral-sh/uv/pull/9756
synced_at: 2026-01-10T12:00:01Z
```

# Rename Python install scratch directory from `.cache` -> `.temp`

---

_Pull request opened by @zanieb on 2024-12-10 00:15_

Addressing the confusion in https://github.com/astral-sh/uv/issues/9749

---

_Label `enhancement` added by @zanieb on 2024-12-10 00:15_

---

_Review requested from @charliermarsh by @zanieb on 2024-12-10 02:53_

---

_Comment by @konstin on 2024-12-10 09:47_

What about `.temp`? This makes it clear to the user that they can safely remove the directory if one remains around.

---

_Renamed from "Rename Python install scratch directory from `.cache` -> `.scratch`" to "Rename Python install scratch directory from `.cache` -> `.temp`" by @zanieb on 2024-12-10 14:29_

---

_Merged by @zanieb on 2024-12-10 14:41_

---

_Closed by @zanieb on 2024-12-10 14:41_

---

_Branch deleted on 2024-12-10 14:41_

---

_@charliermarsh reviewed on 2024-12-10 19:39_

---

_Review comment by @charliermarsh on `crates/uv-python/src/managed.rs`:132 on 2024-12-10 19:39_

I think the rename here is now showing a warning:

```
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `.cache`: not enough `-`-separated values
```

Can you look for the usages? I think we exclude files that match this name somewhere, and now `.cache` exists for users but won't be skipped.

---
