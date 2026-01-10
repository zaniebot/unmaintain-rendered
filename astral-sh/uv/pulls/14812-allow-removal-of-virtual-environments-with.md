```yaml
number: 14812
title: Allow removal of virtual environments with missing interpreters
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/venv-delete
created_at: 2025-07-22T13:50:36Z
updated_at: 2025-07-22T15:17:00Z
url: https://github.com/astral-sh/uv/pull/14812
synced_at: 2026-01-10T06:53:02Z
```

# Allow removal of virtual environments with missing interpreters

---

_Pull request opened by @zanieb on 2025-07-22 13:50_

_No description provided._

---

_Label `bug` added by @zanieb on 2025-07-22 13:50_

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:4832 on 2025-07-22 13:53_

We're too smart for the simple check, we need to delete Python too:

```suggestion
    context.venv().arg("--clear").assert().success();
    fs_err::remove_file(context.temp_dir.join(".venv/pyvenv.cfg"))?;
    fs_err::remove_file(context.temp_dir.join(".venv/bin/python"))?;
    uv_snapshot!(context.filters(), context.sync().arg("-p").arg("3.12").env_remove(EnvVars::VIRTUAL_ENV), @r"
    success: false
    exit_code: 2
    ----- stdout -----

    ----- stderr -----
    warning: Ignoring existing virtual environment linked to non-existent Python interpreter: .venv/bin/python3 -> python
    Using CPython 3.12.[X] interpreter at: [PYTHON-3.12]
    error: Project virtual environment directory `[VENV]/` cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment
    ");
```

---

_@konstin approved on 2025-07-22 13:54_

Thanks!

---

_Merged by @zanieb on 2025-07-22 15:16_

---

_Closed by @zanieb on 2025-07-22 15:16_

---

_Branch deleted on 2025-07-22 15:17_

---
