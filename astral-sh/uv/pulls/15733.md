```yaml
number: 15733
title: Error early for parent path in build backend
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - build-backend
assignees: []
merged: true
base: main
head: konsti/error-on-relative-paths-in-build-backend
created_at: 2025-09-08T12:45:08Z
updated_at: 2025-09-09T13:03:22Z
url: https://github.com/astral-sh/uv/pull/15733
synced_at: 2026-01-10T06:36:15Z
```

# Error early for parent path in build backend

---

_Pull request opened by @konstin on 2025-09-08 12:45_

Paths referencing above the directory of the `pyproject.toml`, such as `module-root = ".."`, are not supported by the build backend. The check that should catch was not working properly, so the source distribution built successfully and only the wheel build failed. We now error early. The same fix is applied to data includes.

Fix #15702

---

_Label `bug` added by @konstin on 2025-09-08 12:45_

---

_Label `build-backend` added by @konstin on 2025-09-08 12:45_

---

_@zanieb approved on 2025-09-08 12:51_

---

_@zanieb reviewed on 2025-09-08 12:51_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/lib.rs`:70 on 2025-09-08 12:51_

I think we tend to omit the backticks following a `:`

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:70 on 2025-09-08 12:57_

I'll change it consistently in a separate PR

---

_@konstin reviewed on 2025-09-08 12:57_

---

_Merged by @konstin on 2025-09-08 13:53_

---

_Closed by @konstin on 2025-09-08 13:53_

---

_Branch deleted on 2025-09-08 13:53_

---

_Comment by @DhavalGojiya on 2025-09-09 13:03_

Thank you for all your hard work, @konstin.

---
