```yaml
number: 10872
title: "Disable `.egg-info` tests via `slow-tests` feature on Windows and macOS"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/egg-info
created_at: 2025-01-22T19:35:35Z
updated_at: 2025-01-22T21:39:05Z
url: https://github.com/astral-sh/uv/pull/10872
synced_at: 2026-01-10T11:45:15Z
```

# Disable `.egg-info` tests via `slow-tests` feature on Windows and macOS

---

_Pull request opened by @charliermarsh on 2025-01-22 19:35_

## Summary

These are super slow on Windows and it's not critical to test them on that platform. Let's just do the lazy thing.


---

_Comment by @zanieb on 2025-01-22 19:49_

We could also add a slow-tests feature? Which could be a better dev-ex locally?

---

_Comment by @charliermarsh on 2025-01-22 20:30_

Sounds good, I'll do that.

---

_Renamed from "Disable `.egg-info` tests on Windows" to "Disable `.egg-info` tests via `slow-tests` feature on Windows and macOS" by @charliermarsh on 2025-01-22 20:54_

---

_@charliermarsh reviewed on 2025-01-22 20:54_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:282 on 2025-01-22 20:54_

Unchanged; just splitting over multiple lines.

---

_@charliermarsh reviewed on 2025-01-22 20:54_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:232 on 2025-01-22 20:54_

Everything except `slow-tests` and `test-ecosystem`.

---

_Label `testing` added by @charliermarsh on 2025-01-22 20:56_

---

_@zanieb reviewed on 2025-01-22 20:58_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:282 on 2025-01-22 20:58_

This is PowerShell — you can't split lines like that.

I think you need to use ` or switch the shell to bash (the latter is probably for the best)

---

_@zanieb approved on 2025-01-22 21:01_

---

_Merged by @charliermarsh on 2025-01-22 21:39_

---

_Closed by @charliermarsh on 2025-01-22 21:39_

---

_Branch deleted on 2025-01-22 21:39_

---
