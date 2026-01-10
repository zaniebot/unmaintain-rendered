```yaml
number: 11208
title: Use base Python for cached environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/base
created_at: 2025-02-04T01:21:16Z
updated_at: 2025-02-04T22:23:09Z
url: https://github.com/astral-sh/uv/pull/11208
synced_at: 2026-01-10T11:10:34Z
```

# Use base Python for cached environments

---

_Pull request opened by @charliermarsh on 2025-02-04 01:21_

## Summary

It turns out that we were returning slightly different interpreter paths on repeated `uv run --with` commands. This likely didn't affect many (or any?) users, but it does affect our test suite, since in the test suite, we use a symlinked interpreter.

The issue is that on first invocation, we create the virtual environment, and that returns the path to the `python` executable in the environment. On second invocation, we return the `python3` executable, since that gets priority during discovery. This on its own is potentially ok. The issue is that these resolve to different `sys._base_executable` values in these flows... The latter gets the correct value (since it's read from the `home` key), but the former gets the incorrect value (since it's just the `base_executable` of the executable that created the virtualenv, which is the symlink).

We now use the same logic to determine the "cached interpreter" as in virtual environment creation, to ensure consistency between those paths.


---

_Review requested from @zanieb by @charliermarsh on 2025-02-04 01:21_

---

_Label `bug` added by @charliermarsh on 2025-02-04 01:21_

---

_@charliermarsh reviewed on 2025-02-04 01:21_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/run.rs`:3925 on 2025-02-04 01:21_

On `main`, this re-installs `iniconfig`.

---

_@charliermarsh reviewed on 2025-02-04 01:22_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:127 on 2025-02-04 01:22_

Everything in this file is just moved from `uv-virtualenv`.

---

_@zanieb approved on 2025-02-04 22:04_

---

_Merged by @charliermarsh on 2025-02-04 22:23_

---

_Closed by @charliermarsh on 2025-02-04 22:23_

---

_Branch deleted on 2025-02-04 22:23_

---
