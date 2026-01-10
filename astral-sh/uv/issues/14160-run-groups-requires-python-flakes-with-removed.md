---
number: 14160
title: "`run_groups_requires_python` flakes with `Removed virtual environment at ...`"
type: issue
state: closed
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-06-20T15:36:03Z
updated_at: 2025-06-30T15:39:48Z
url: https://github.com/astral-sh/uv/issues/14160
synced_at: 2026-01-10T01:25:43Z
---

# `run_groups_requires_python` flakes with `Removed virtual environment at ...`

---

_Issue opened by @zanieb on 2025-06-20 15:36_

```
    running 1 test
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: run_groups_requires_python-4
    Source: crates/uv/tests/it/run.rs:4690
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
              5 │+Using CPython 3.12.[X] interpreter at: [PYTHON-3.12]
              6 │+Removed virtual environment at: .venv
              7 │+Creating virtual environment at: .venv
        5     8 │ Resolved 6 packages in [TIME]
        6       │-Audited 2 packages in [TIME]
              9 │+Installed 2 packages in [TIME]
             10 │+ + sniffio==1.3.1
             11 │+ + typing-extensions==4.10.0
    ────────────┴───────────────────────────────────────────────────────────────────
    Stopped on the first failure. Run `cargo insta test` to run all snapshots.
    test run::run_groups_requires_python ... FAILED
```

---

_Label `ci-flake` added by @zanieb on 2025-06-20 15:36_

---

_Comment by @zanieb on 2025-06-20 15:36_

Presumably related to 

- #13745 
- #13744 

---

_Comment by @Gankra on 2025-06-20 15:53_

Just hit this in https://github.com/astral-sh/uv/actions/runs/15782642131/job/44491853582?pr=14161

---

_Comment by @zanieb on 2025-06-20 16:43_

Hm @oconnor663 hit this too in https://github.com/astral-sh/uv/pull/14153#issuecomment-2992025139

Maybe this is a new problem?

---

_Comment by @Gankra on 2025-06-20 18:12_

It's a brand-new test from #13735 (merged last week) so it's certainly possible it's acutely problematic. slams the heck out of `uv run python` too.

---

_Referenced in [astral-sh/uv#14153](../../astral-sh/uv/pulls/14153.md) on 2025-06-20 18:24_

---

_Comment by @jtfmumm on 2025-06-21 09:32_

The first time it shows up is a couple commits after #13735 on #14047 so it may have been an existing issue.

---

_Referenced in [astral-sh/uv#14275](../../astral-sh/uv/pulls/14275.md) on 2025-06-26 14:23_

---

_Referenced in [astral-sh/uv#14331](../../astral-sh/uv/pulls/14331.md) on 2025-06-27 20:35_

---

_Closed by @zanieb on 2025-06-30 15:39_

---

_Referenced in [astral-sh/uv#16165](../../astral-sh/uv/issues/16165.md) on 2025-10-07 22:16_

---
