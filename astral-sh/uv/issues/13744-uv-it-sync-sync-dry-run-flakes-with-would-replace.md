---
number: 13744
title: "`uv::it sync::sync_dry_run` flakes with \"Would replace existing virtual environment\""
type: issue
state: closed
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-05-30T21:11:21Z
updated_at: 2025-06-30T15:39:49Z
url: https://github.com/astral-sh/uv/issues/13744
synced_at: 2026-01-10T01:25:38Z
---

# `uv::it sync::sync_dry_run` flakes with "Would replace existing virtual environment"

---

_Issue opened by @zanieb on 2025-05-30 21:11_

``` 
       FAIL [   2.647s] uv::it sync::sync_dry_run
──── STDOUT:             uv::it sync::sync_dry_run

running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: sync_dry_run-6
Source: crates/uv/tests/it/sync.rs:7883
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

failures:

failures:
    sync::sync_dry_run

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 1987 filtered out; finished in 2.64s

──── STDERR:             uv::it sync::sync_dry_run

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.12.9 interpreter at: /home/runner/.local/share/uv/tests/.tmpmwHV95/python/3.12/python3
Would create virtual environment at: .venv
Resolved 2 packages in 104ms
Would create lockfile at: uv.lock
Would download 1 package
Would install 1 package
 + iniconfig==2.0.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.12.9 interpreter at: /home/runner/.local/share/uv/tests/.tmpmwHV95/python/3.12/python3
Creating virtual environment at: .venv
Resolved 2 packages in 62ms
Prepared 1 package in 24ms
Installed 1 package in 8ms
 + iniconfig==2.0.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Discovered existing environment at: .venv
Resolved 2 packages in 97ms
Would update lockfile at: uv.lock
Would download 1 package
Would uninstall 1 package
Would install 1 package
 - iniconfig==2.0.0
 + typing-extensions==4.10.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.8.20 interpreter at: /home/runner/.local/share/uv/tests/.tmpmwHV95/python/3.8/python3
Would replace existing virtual environment at: .venv
Resolved 2 packages in 61ms
Would update lockfile at: uv.lock
Would install 1 package
 + iniconfig==2.0.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.8.20 interpreter at: /home/runner/.local/share/uv/tests/.tmpmwHV95/python/3.8/python3
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 2 packages in 54ms
Installed 1 package in 8ms
 + iniconfig==2.0.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.8.20 interpreter at: /home/runner/.local/share/uv/tests/.tmpmwHV95/python/3.8/python3
Would replace existing virtual environment at: .venv
Resolved 2 packages in 46ms
Found up-to-date lockfile at: uv.lock
Would install 1 package
 + iniconfig==2.0.0

────────────────────────────────────────────────────────────────────────────────
```

https://github.com/astral-sh/uv/actions/runs/15355564391/job/43213731634?pr=13743

---

_Comment by @zanieb on 2025-05-30 21:12_

In the same run, `sync_locked_script` failed too:

```
        FAIL [   2.756s] uv::it sync::sync_locked_script
──── STDOUT:             uv::it sync::sync_locked_script

running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: sync_locked_script-8
Source: crates/uv/tests/it/sync.rs:8381
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
    5       │-Using script environment at: [CACHE_DIR]/environments-v2/script-[HASH]
          5 │+Recreating script environment at: [CACHE_DIR]/environments-v2/script-[HASH]
    6     6 │ Resolved 6 packages in [TIME]
    7     7 │ Prepared 2 packages in [TIME]
    8     8 │ Installed 6 packages in [TIME]
    9     9 │  + anyio==4.3.0
────────────┴───────────────────────────────────────────────────────────────────
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
test sync::sync_locked_script ... FAILED

failures:

failures:
    sync::sync_locked_script

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 1987 filtered out; finished in 2.75s

──── STDERR:             uv::it sync::sync_locked_script

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 3 packages in 135ms

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Creating script environment at: /home/runner/.local/share/uv/tests/.tmpAaXiHH/cache/environments-v2/script-41d1daf96148b5f0
Resolved 3 packages in 58ms
Prepared 3 packages in 52ms
Installed 3 packages in 14ms
 + anyio==4.3.0
 + idna==3.6
 + sniffio==1.3.1

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using script environment at: /home/runner/.local/share/uv/tests/.tmpAaXiHH/cache/environments-v2/script-41d1daf96148b5f0
Resolved 4 packages in 118ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using script environment at: /home/runner/.local/share/uv/tests/.tmpAaXiHH/cache/environments-v2/script-41d1daf96148b5f0
Resolved 4 packages in 94ms
Prepared 1 package in 20ms
Installed 1 package in 11ms
 + iniconfig==2.0.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Recreating script environment at: /home/runner/.local/share/uv/tests/.tmpAaXiHH/cache/environments-v2/script-41d1daf96148b5f0
Resolved 6 packages in 124ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Recreating script environment at: /home/runner/.local/share/uv/tests/.tmpAaXiHH/cache/environments-v2/script-41d1daf96148b5f0
Resolved 6 packages in 57ms
Prepared 2 packages in 30ms
Installed 6 packages in 9ms
 + anyio==4.3.0
 + exceptiongroup==1.2.0
 + idna==3.6
 + iniconfig==2.0.0
 + sniffio==1.3.1
 + typing-extensions==4.10.0

────────────────────────────────────────────────────────────────────────────────


thread 'sync::sync_locked_script' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/insta-1.43.1/src/runtime.rs:679:13:
snapshot assertion for 'sync_locked_script-8' failed in line 8381
```

---

_Comment by @zanieb on 2025-05-30 21:13_

and `sync_script`

```

        FAIL [   2.424s] uv::it sync::sync_script
──── STDOUT:             uv::it sync::sync_script

running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: sync_script-5
Source: crates/uv/tests/it/sync.rs:8128
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    1     1 │ exit_code: 2
    2     2 │ ----- stdout -----
    3     3 │ 
    4     4 │ ----- stderr -----
    5       │-Using script environment at: [CACHE_DIR]/environments-v2/script-[HASH]
          5 │+Recreating script environment at: [CACHE_DIR]/environments-v2/script-[HASH]
    6     6 │ error: `uv sync --locked` requires a script lockfile; run `uv lock --script script.py` to lock the script
────────────┴───────────────────────────────────────────────────────────────────
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
test sync::sync_script ... FAILED

failures:

failures:
    sync::sync_script

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 1987 filtered out; finished in 2.41s

──── STDERR:             uv::it sync::sync_script

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Creating script environment at: /home/runner/.local/share/uv/tests/.tmpnaboah/cache/environments-v2/script-8d98b426ac680738
Resolved 3 packages in 84ms
Prepared 3 packages in 33ms
Installed 3 packages in 12ms
 + anyio==4.3.0
 + idna==3.6
 + sniffio==1.3.1

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using script environment at: /home/runner/.local/share/uv/tests/.tmpnaboah/cache/environments-v2/script-8d98b426ac680738
Resolved 4 packages in 38ms
Prepared 1 package in 5ms
Installed 1 package in 6ms
 + iniconfig==2.0.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using script environment at: /home/runner/.local/share/uv/tests/.tmpnaboah/cache/environments-v2/script-8d98b426ac680738
Resolved 3 packages in 13ms
Uninstalled 1 package in 0.70ms
 - iniconfig==2.0.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Recreating script environment at: /home/runner/.local/share/uv/tests/.tmpnaboah/cache/environments-v2/script-8d98b426ac680738
Resolved 5 packages in 78ms
Prepared 2 packages in 16ms
Installed 5 packages in 9ms
 + anyio==4.3.0
 + exceptiongroup==1.2.0
 + idna==3.6
 + sniffio==1.3.1
 + typing-extensions==4.10.0

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Recreating script environment at: /home/runner/.local/share/uv/tests/.tmpnaboah/cache/environments-v2/script-8d98b426ac680738
error: `uv sync --locked` requires a script lockfile; run `uv lock --script script.py` to lock the script

────────────────────────────────────────────────────────────────────────────────


thread 'sync::sync_script' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/insta-1.43.1/src/runtime.rs:679:13:
snapshot assertion for 'sync_script-5' failed in line 8128
```

---

_Referenced in [astral-sh/uv#13745](../../astral-sh/uv/issues/13745.md) on 2025-05-30 21:15_

---

_Label `ci-flake` added by @zanieb on 2025-05-30 21:16_

---

_Comment by @zanieb on 2025-06-03 14:14_

Another occurance

```
 SLOW [  12.748s] uv::it python_install::python_reinstall_patch
        FAIL [   2.858s] uv::it sync::sync_dry_run
  stdout ───

    running 1 test
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: sync_dry_run-6
    Source: crates/uv/tests/it/sync.rs:7883
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

    failures:

    failures:
        sync::sync_dry_run

    test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 1989 filtered out; finished in 2.85s
    
  stderr ───

    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ----- stdout -----

    ----- stderr -----
    Using CPython 3.12.9 interpreter at: /home/runner/.local/share/uv/tests/.tmp9tGjqW/python/3.12/python3
    Would create virtual environment at: .venv
    Resolved 2 packages in 95ms
    Would create lockfile at: uv.lock
    Would download 1 package
    Would install 1 package
     + iniconfig==2.0.0

    ────────────────────────────────────────────────────────────────────────────────


    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ----- stdout -----

    ----- stderr -----
    Using CPython 3.12.9 interpreter at: /home/runner/.local/share/uv/tests/.tmp9tGjqW/python/3.12/python3
    Creating virtual environment at: .venv
    Resolved 2 packages in 70ms
    Prepared 1 package in 15ms
    Installed 1 package in 8ms
     + iniconfig==2.0.0

    ────────────────────────────────────────────────────────────────────────────────


    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ----- stdout -----

    ----- stderr -----
    Discovered existing environment at: .venv
    Resolved 2 packages in 100ms
    Would update lockfile at: uv.lock
    Would download 1 package
    Would uninstall 1 package
    Would install 1 package
     - iniconfig==2.0.0
     + typing-extensions==4.10.0

    ────────────────────────────────────────────────────────────────────────────────


    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ----- stdout -----

    ----- stderr -----
    Using CPython 3.8.20 interpreter at: /home/runner/.local/share/uv/tests/.tmp9tGjqW/python/3.8/python3
    Would replace existing virtual environment at: .venv
    Resolved 2 packages in 57ms
    Would update lockfile at: uv.lock
    Would install 1 package
     + iniconfig==2.0.0

    ────────────────────────────────────────────────────────────────────────────────


    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ----- stdout -----

    ----- stderr -----
    Using CPython 3.8.20 interpreter at: /home/runner/.local/share/uv/tests/.tmp9tGjqW/python/3.8/python3
    Removed virtual environment at: .venv
    Creating virtual environment at: .venv
    Resolved 2 packages in 52ms
    Installed 1 package in 14ms
     + iniconfig==2.0.0

    ────────────────────────────────────────────────────────────────────────────────


    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ----- stdout -----

    ----- stderr -----
    Using CPython 3.8.20 interpreter at: /home/runner/.local/share/uv/tests/.tmp9tGjqW/python/3.8/python3
    Would replace existing virtual environment at: .venv
    Resolved 2 packages in 49ms
    Found up-to-date lockfile at: uv.lock
    Would install 1 package
     + iniconfig==2.0.0

    ────────────────────────────────────────────────────────────────────────────────


    thread 'sync::sync_dry_run' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/insta-1.43.1/src/runtime.rs:679:13:
    snapshot assertion for 'sync_dry_run-6' failed in line 7883
    note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Notably, this one failed by itself.

---

_Referenced in [astral-sh/uv#13817](../../astral-sh/uv/pulls/13817.md) on 2025-06-11 13:51_

---

_Referenced in [astral-sh/uv#14160](../../astral-sh/uv/issues/14160.md) on 2025-06-20 15:36_

---

_Referenced in [astral-sh/uv#14331](../../astral-sh/uv/pulls/14331.md) on 2025-06-27 20:35_

---

_Closed by @zanieb on 2025-06-30 15:39_

---
