```yaml
number: 14326
title: "Cache Python downloads by default in `python install` tests"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/py-install-cache
created_at: 2025-06-27T19:25:53Z
updated_at: 2025-07-28T17:33:58Z
url: https://github.com/astral-sh/uv/pull/14326
synced_at: 2026-01-12T16:11:09Z
```

# Cache Python downloads by default in `python install` tests

---

_@zanieb_

Adds a cache bucket for Python installs and uses it by default during tests, extending the opt-in cache added in https://github.com/astral-sh/uv/pull/12175

Updates the `python_install` tests to use a shared cache for Python installs. This reduces the `python_install` test runtime on my machine from 23s -> 17s. The difference should be much larger on machines with slower internet and less cores for test workers :) This should also improve stability in CI by reducing reliance on the network during test runs, see #14327

---

_Label `testing` added by @zanieb on 2025-06-27 19:25_

---

_Renamed from "Cache Python downloads during install tests" to "Cache Python downloads by default and use during `python install` tests" by @zanieb on 2025-07-18 12:52_

---

_Marked ready for review by @zanieb on 2025-07-23 12:41_

---

_Renamed from "Cache Python downloads by default and use during `python install` tests" to "Cache Python downloads by default in `python install` tests" by @zanieb on 2025-07-23 16:51_

---

_@geofft approved on 2025-07-28 17:16_

---

_Merged by @zanieb on 2025-07-28 17:33_

---

_Closed by @zanieb on 2025-07-28 17:33_

---

_Branch deleted on 2025-07-28 17:33_

---
