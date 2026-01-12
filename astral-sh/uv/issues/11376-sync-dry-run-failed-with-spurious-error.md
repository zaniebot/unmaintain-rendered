```yaml
number: 11376
title: "`sync_dry_run` failed with spurious error"
type: issue
state: closed
author: charliermarsh
labels:
  - testing
assignees: []
created_at: 2025-02-10T03:13:16Z
updated_at: 2025-07-11T02:36:09Z
url: https://github.com/astral-sh/uv/issues/11376
synced_at: 2026-01-12T16:00:35Z
```

# `sync_dry_run` failed with spurious error

---

_@charliermarsh_

![Image](https://github.com/user-attachments/assets/872c0d82-e2bb-404f-ba3c-ef41bd806a35)

---

_Label `testing` added by @charliermarsh on 2025-02-10 03:13_

---

_Comment by @zanieb on 2025-02-10 21:33_

```
running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: sync_dry_run-4
Source: crates/uv/tests/it/sync.rs:6647
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3     3 │ 
    4     4 │ ----- stderr -----
    5       │-Discovered existing environment at: .venv
          5 │+Using CPython 3.8.[X] interpreter at: [PYTHON-3.8]
          6 │+Would replace existing virtual environment at: .venv
    6     7 │ Resolved 2 packages in [TIME]
    7     8 │ Found up-to-date lockfile at: uv.lock
    8       │-Audited 1 package in [TIME]
    9       │-Would make no changes
          9 │+Would install 1 package
         10 │+ + iniconfig==2.0.0
────────────┴───────────────────────────────────────────────────────────────────
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
test sync::sync_dry_run ... FAILED
```

Ubuntu https://github.com/astral-sh/uv/actions/runs/13250788244/job/36987964537?pr=11405

---

_Comment by @charliermarsh on 2025-02-10 21:41_

I haven't been able to reproduce this locally, but it might be helpful to show the snapshots for the preceding commands (which are currently just asserted).

---

_Comment by @charliermarsh on 2025-04-28 15:00_

These are back with some frequency: https://github.com/astral-sh/uv/actions/runs/14710321658/job/41280823131?pr=13176

---

_Comment by @charliermarsh on 2025-07-11 02:36_

Oh, we think we fixed this.

---

_Closed by @charliermarsh on 2025-07-11 02:36_

---
