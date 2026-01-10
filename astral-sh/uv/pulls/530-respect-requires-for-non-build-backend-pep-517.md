```yaml
number: 530
title: "Respect `requires` for non-`build-backend` PEP 517 builds"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/backend
created_at: 2023-12-04T01:18:20Z
updated_at: 2023-12-04T10:13:43Z
url: https://github.com/astral-sh/uv/pull/530
synced_at: 2026-01-10T15:44:44Z
```

# Respect `requires` for non-`build-backend` PEP 517 builds

---

_Pull request opened by @charliermarsh on 2023-12-04 01:18_

## Summary

This PR modifies `puffin-build` to be closer in behavior to [pip](https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/pyproject.py#L53) and [build](https://github.com/pypa/build/blob/de5b44b0c28c598524832dff685a98d5a5148c44/src/build/__init__.py#L94).

Specifically, if a project contains a `[build-system]` field, but no `build-backend`, we now perform a PEP 517 build (instead of using `setup.py` directly) _and_ respect the `requires` of the `[build-system]`. Without this change, we were failing to build source distributions for packages like `ujson`.

Closes #527.


---

_Review requested from @konstin by @charliermarsh on 2023-12-04 01:18_

---

_Label `bug` added by @charliermarsh on 2023-12-04 01:18_

---

_Renamed from "Use build backend" to "Respect `requires` for non-`build-backend` PEP 517 builds" by @charliermarsh on 2023-12-04 01:18_

---

_@charliermarsh reviewed on 2023-12-04 01:19_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_sync.rs`:929 on 2023-12-04 01:19_

(This test passed prior to this PR.)

---

_@charliermarsh reviewed on 2023-12-04 01:19_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_sync.rs`:893 on 2023-12-04 01:19_

(This test failed prior to this PR, because `wheel` isn't included in the `requires`, but _is_ required to build. The fix is to ensure we use a PEP 517 build, so that the `setuptools` dependencies get pulled in.)

---

_@konstin approved on 2023-12-04 10:10_

---

_Merged by @konstin on 2023-12-04 10:13_

---

_Closed by @konstin on 2023-12-04 10:13_

---

_Branch deleted on 2023-12-04 10:13_

---
