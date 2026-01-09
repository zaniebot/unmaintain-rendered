---
number: 8681
title: "CI flakiness: run_from_directory"
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2024-10-29T19:20:58Z
updated_at: 2024-10-29T20:03:21Z
url: https://github.com/astral-sh/uv/issues/8681
synced_at: 2026-01-07T13:12:18-06:00
---

# CI flakiness: run_from_directory

---

_Issue opened by @konstin on 2024-10-29 19:20_

`run_from_directory` sometimes recreates the venv: https://github.com/astral-sh/uv/actions/runs/11578473249/job/32232394993

> --- STDOUT:              uv::it run::run_from_directory ---
> 
> running 1 test
> ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
> Snapshot: run_from_directory-7
> Source: crates/uv/tests/it/run.rs:1823
> ────────────────────────────────────────────────────────────────────────────────
> Expression: snapshot
> ────────────────────────────────────────────────────────────────────────────────
> -old snapshot
> +new results
> ────────────┬───────────────────────────────────────────────────────────────────
>     3     3 │ 3.10.[X]
>     4     4 │ 
>     5     5 │ ----- stderr -----
>     6     6 │ warning: `VIRTUAL_ENV=[VENV]/` does not match the project environment path `.venv` and will be ignored
>           7 │+Using CPython 3.10.[X] interpreter at: [PYTHON-3.10]
>           8 │+Removed virtual environment at: .venv
>           9 │+Creating virtual environment at: .venv
>     7    10 │ Resolved 1 package in [TIME]
>     8       │-Audited 1 package in [TIME]
>          11 │+Installed 1 package in [TIME]
>          12 │+ + foo==1.0.0 (from file://[TEMP_DIR]/project)
> ────────────┴───────────────────────────────────────────────────────────────────
> Stopped on the first failure. Run `cargo insta test` to run all snapshots.
> test run::run_from_directory ... FAILED

---

_Label `internal` added by @konstin on 2024-10-29 19:20_

---

_Comment by @zanieb on 2024-10-29 20:03_

Isn't this the one Charlie just fixed in #8673 ?

---

_Comment by @konstin on 2024-10-29 20:03_

Thank you!

---

_Closed by @konstin on 2024-10-29 20:03_

---
