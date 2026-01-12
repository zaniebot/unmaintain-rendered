```yaml
number: 11415
title: "Mark `sync::sync_dry_run` test as requiring `python-managed`"
type: pull_request
state: merged
author: mgorny
labels:
  - internal
assignees: []
merged: true
base: main
head: dry-run-mark
created_at: 2025-02-11T07:28:25Z
updated_at: 2025-02-11T17:35:25Z
url: https://github.com/astral-sh/uv/pull/11415
synced_at: 2026-01-12T16:09:50Z
```

# Mark `sync::sync_dry_run` test as requiring `python-managed`

---

_@mgorny_

## Summary

Mark `sync::sync_dry_run` as requiring `python-managed`, as it fails with system Python executables due to extraneous Python packages being installed:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: sync_dry_run
Source: crates/uv/tests/it/sync.rs:6574
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    5     5 │ Using CPython 3.12.[X] interpreter at: [PYTHON-3.12]
    6     6 │ Would create virtual environment at: .venv
    7     7 │ Resolved 2 packages in [TIME]
    8     8 │ Would create lockfile at: uv.lock
    9       │-Would download 1 package
   10       │-Would install 1 package
   11       │- + iniconfig==2.0.0
          9 │+Would uninstall 1310 packages
         10 │+ - a2wsgi==1.10.8
[…]
```

## Test Plan

```
cargo test --no-default-features --features=python,pypi,git
```

---

_Comment by @mgorny on 2025-02-11 08:00_

Looks like macOS failed due to a flaky test.

---

_Label `internal` added by @konstin on 2025-02-11 08:42_

---

_@konstin approved on 2025-02-11 08:42_

---

_Merged by @konstin on 2025-02-11 08:55_

---

_Closed by @konstin on 2025-02-11 08:55_

---

_Branch deleted on 2025-02-11 09:17_

---

_Comment by @mgorny on 2025-02-11 09:17_

Thanks!

---

_Comment by @zanieb on 2025-02-11 15:11_

This seems wrong... cc @charliermarsh 

Related #11376 

---

_Comment by @zanieb on 2025-02-11 15:13_

It looks like we say

https://github.com/astral-sh/uv/blob/0735be21b92c695ded494d6e3b4be16b60e7c993/crates/uv/tests/it/sync.rs#L6650

then perform the sync dry-run against the interpreter's environment instead of an empty one?

---

_Comment by @mgorny on 2025-02-11 17:29_

Oops, so I've found a serious bug? :-)

---

_Comment by @charliermarsh on 2025-02-11 17:35_

Haha, well it's just dry-run reporting (doesn't modify the environment at all), so not _serious_, but definitely wrong.

---
