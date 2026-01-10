```yaml
number: 12884
title: Ensure virtual environment is compatible with interpreter on sync
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/venv-compatibility
created_at: 2025-04-14T18:10:48Z
updated_at: 2025-04-15T10:01:16Z
url: https://github.com/astral-sh/uv/pull/12884
synced_at: 2026-01-10T11:10:40Z
```

# Ensure virtual environment is compatible with interpreter on sync

---

_Pull request opened by @jtfmumm on 2025-04-14 18:10_

It was possible that a virtual environment became out of sync with the interpreter it pointed to (for example, if a symlink was changed to an updated Python version). In such a case, `pyvenv.cfg` and `activate_this.py` would no longer be correct. This PR detects when the `version` (`venv` module) or `version_info` (uv and `virtualenv`) field in `pyvenv.cfg` is out of sync with the interpreter. In such a case, uv recreates the virtual environment.

Closes #12461


---

_Review requested from @konstin by @jtfmumm on 2025-04-14 18:11_

---

_Label `bug` added by @jtfmumm on 2025-04-14 18:18_

---

_Review comment by @konstin on `crates/uv-python/src/environment.rs`:361 on 2025-04-15 08:55_

This would be an odd situation since we are in a venv exactly if pyvenv.cfg exists (not sure if that is actionable)

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:806 on 2025-04-15 09:03_

Can we have a dedicated log message for venv-interpreter-version-mismatch?

---

_@konstin approved on 2025-04-15 09:05_

---

_@jtfmumm reviewed on 2025-04-15 09:29_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/mod.rs`:806 on 2025-04-15 09:29_

Added

---

_Review comment by @jtfmumm on `crates/uv-python/src/environment.rs`:361 on 2025-04-15 09:29_

I updated the comment to indicate that **if** this is a virtual environment, we compare the `pyvenv.cfg` version. Otherwise, we return `true`.

---

_@jtfmumm reviewed on 2025-04-15 09:29_

---

_Merged by @jtfmumm on 2025-04-15 10:01_

---

_Closed by @jtfmumm on 2025-04-15 10:01_

---

_Branch deleted on 2025-04-15 10:01_

---
