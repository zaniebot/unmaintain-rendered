```yaml
number: 12240
title: Support modules with different casing in build backend
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/build-backend-normalize-module-name
created_at: 2025-03-17T14:37:54Z
updated_at: 2025-03-23T13:29:22Z
url: https://github.com/astral-sh/uv/pull/12240
synced_at: 2026-01-10T11:10:39Z
```

# Support modules with different casing in build backend

---

_Pull request opened by @konstin on 2025-03-17 14:37_

Match the module name to its module directory with potentially different casing.

For example, a package may have the dist-info-normalized package name `pil_util`, but the importable module is named `PIL_util`.

We get the module name either as dist-info-normalized package name, or explicitly from the user. For dist-info-normalizing a package name, the rules are lowercasing, replacing `.` with `_` and replace `-` with `_`. Since `.` and `-` are not allowed in module names, we can check whether a directory name matches our expected module name by lowercasing it.

Fixes #12187


---

_Label `bug` added by @konstin on 2025-03-17 14:37_

---

_@charliermarsh reviewed on 2025-03-17 18:01_

---

_Review comment by @charliermarsh on `crates/uv-build-backend/src/wheel.rs`:257 on 2025-03-17 18:01_

You can probably use `to_str` here since if it's not Unicode it would never match, right?

---

_@charliermarsh reviewed on 2025-03-17 18:02_

---

_Review comment by @charliermarsh on `crates/uv-build-backend/src/wheel.rs`:266 on 2025-03-17 18:02_

I would suggest adding an error for the case in which a matching directory exists, but there's no `__init__.py` file.

---

_Comment by @NourEldin-Osama on 2025-03-21 03:32_

May I ask when this pull request will get merged?

---

_Review requested from @charliermarsh by @konstin on 2025-03-22 01:17_

---

_@charliermarsh approved on 2025-03-23 13:17_

---

_Merged by @charliermarsh on 2025-03-23 13:29_

---

_Closed by @charliermarsh on 2025-03-23 13:29_

---

_Branch deleted on 2025-03-23 13:29_

---
