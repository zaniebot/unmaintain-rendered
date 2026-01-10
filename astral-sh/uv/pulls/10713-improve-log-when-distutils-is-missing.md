```yaml
number: 10713
title: Improve log when distutils is missing
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/distutils
created_at: 2025-01-17T15:14:04Z
updated_at: 2025-01-20T17:29:30Z
url: https://github.com/astral-sh/uv/pull/10713
synced_at: 2026-01-10T11:45:05Z
```

# Improve log when distutils is missing

---

_Pull request opened by @zanieb on 2025-01-17 15:14_

See https://github.com/astral-sh/uv/issues/4204 for motivation

This doesn't really reach the user experience I'd expect — i.e., we end up saying a virtual environment "does not exist" which is a little silly. However, I think improving the error messaging on interpreter queries in general should be solved separately. I did one small "general" change in https://github.com/astral-sh/uv/pull/10713/commits/89e11d022222a999a4ba8eb73397ea314a636811 — otherwise we don't show the message at all.

---

_Renamed from "Display better error when distutils is missing" to "Improve log when distutils is missing" by @zanieb on 2025-01-17 15:51_

---

_Review comment by @zanieb on `crates/uv-python/python/get_interpreter_info.py`:404 on 2025-01-17 15:54_

I needed to move this out of `get_scheme` or `get_virtualenv` would fail on distutils import.

---

_@zanieb reviewed on 2025-01-17 15:54_

---

_Marked ready for review by @zanieb on 2025-01-17 16:07_

---

_Label `tracing` added by @zanieb on 2025-01-17 16:07_

---

_Review requested from @konstin by @zanieb on 2025-01-17 16:37_

---

_Comment by @zanieb on 2025-01-17 18:33_

With #10716, we'll should show this as an error instead of a log.

---

_Review comment by @konstin on `.github/workflows/ci.yml`:683 on 2025-01-20 08:36_

```suggestion
  integration-test-deadsnakes-39-linux:
```

---

_@konstin approved on 2025-01-20 08:38_

---

_Merged by @zanieb on 2025-01-20 17:29_

---

_Closed by @zanieb on 2025-01-20 17:29_

---

_Branch deleted on 2025-01-20 17:29_

---
