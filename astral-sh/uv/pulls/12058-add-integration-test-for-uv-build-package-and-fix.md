```yaml
number: 12058
title: "Add integration test for `uv_build` package and fix invocation"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/test-uv-build-package
created_at: 2025-03-07T21:51:50Z
updated_at: 2025-03-07T23:36:14Z
url: https://github.com/astral-sh/uv/pull/12058
synced_at: 2026-01-12T16:10:07Z
```

# Add integration test for `uv_build` package and fix invocation

---

_@konstin_

I somehow missed running an actual integration test of the PEP 517 API in CI and the python shim was using the old uv CLI interface still.

The tests include pip, uv and `python -m build`. They must be a in CI job since we can't depend on the Python package in the Rust tests (we only get the binary in `cargo test`, not the `uv_build` wheel).

---

_Label `bug` added by @konstin on 2025-03-07 21:51_

---

_Label `preview` added by @konstin on 2025-03-07 21:51_

---

_@zanieb reviewed on 2025-03-07 21:53_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1373 on 2025-03-07 21:53_

Why do we need the full git history here?

---

_@zanieb approved on 2025-03-07 21:55_

---

_@zanieb reviewed on 2025-03-07 21:56_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1398 on 2025-03-07 21:56_

Should we also test the `build` "official" frontend?

---

_@zanieb reviewed on 2025-03-07 21:56_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1401 on 2025-03-07 21:56_

Should we test it works here?

---

_@zanieb reviewed on 2025-03-07 21:56_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1401 on 2025-03-07 21:56_

(i..e, after installation?)

---

_Review comment by @konstin on `.github/workflows/ci.yml`:1373 on 2025-03-07 22:02_

Thanks, TIL `fetch-depth: 0` fetches everything instead of setting the lowest possible fetch amount.

---

_@konstin reviewed on 2025-03-07 22:02_

---

_Merged by @konstin on 2025-03-07 22:40_

---

_Closed by @konstin on 2025-03-07 22:40_

---

_Branch deleted on 2025-03-07 22:40_

---

_@zanieb reviewed on 2025-03-07 22:40_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1373 on 2025-03-07 22:40_

Oh lol, yeah.

---

_Renamed from "Integration test uv_build package" to "Add integration test for `uv_build` package and fix invocation" by @zanieb on 2025-03-07 23:36_

---
