---
number: 13928
title: "Flaky test: Unexpected error code on Windows run of `lock_invalid_hash`"
type: issue
state: closed
author: jtfmumm
labels:
  - ci-flake
assignees: []
created_at: 2025-06-09T17:11:54Z
updated_at: 2025-06-09T17:20:25Z
url: https://github.com/astral-sh/uv/issues/13928
synced_at: 2026-01-10T01:25:40Z
---

# Flaky test: Unexpected error code on Windows run of `lock_invalid_hash`

---

_Issue opened by @jtfmumm on 2025-06-09 17:11_

See https://github.com/astral-sh/uv/actions/runs/15539150456/job/43745497711?pr=13917. This happened once but succeeded on subsequent runs.

Error code `-1073741819` might be a Windows Access Violation. 

`lock::lock_invalid_hash` failed with:

```
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: lock_invalid_hash-2
    Source: D:\uv:6607
    ───────────────────────────────────────────────────────────────────────────────
    Expression: snapshot
    ───────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬──────────────────────────────────────────────────────────────────
        0     0 │ success: false
        1       │-exit_code: 1
              1 │+exit_code: -1073741819
        2     2 │ ----- stdout -----
        3     3 │ 
        4     4 │ ----- stderr -----
        5     5 │   × Failed to download `idna==3.6`
    ────────────┴──────────────────────────────────────────────────────────────────
```

---

_Label `ci-flake` added by @jtfmumm on 2025-06-09 17:11_

---

_Comment by @zanieb on 2025-06-09 17:13_

Duplicate of https://github.com/astral-sh/uv/issues/13812

---

_Closed by @jtfmumm on 2025-06-09 17:20_

---
