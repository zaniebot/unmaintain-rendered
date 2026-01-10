```yaml
number: 12799
title: python_list tests fail with Python 3.12.10
type: issue
state: closed
author: mgorny
labels:
  - bug
  - testing
assignees: []
created_at: 2025-04-10T03:00:45Z
updated_at: 2025-04-14T12:23:11Z
url: https://github.com/astral-sh/uv/issues/12799
synced_at: 2026-01-10T03:41:47Z
```

# python_list tests fail with Python 3.12.10

---

_Issue opened by @mgorny on 2025-04-10 03:00_

### Summary

With 0.6.14, I'm seeing the following test failures when testing against system Pythons (`git,pypi,python` features):

```
failures:

---- python_list::python_list_duplicate_path_entries stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
cpython-3.12.10-linux-x86_64-gnu    /var/tmp/portage/dev-python/uv-0.6.14/homedir/.local/share/uv/tests/.tmpPjVbSH/python/3.12/python3 
-> /usr/lib/python-exec/python3.12/python3
cpython-3.11.12-linux-x86_64-gnu    /var/tmp/portage/dev-python/uv-0.6.14/homedir/.local/share/uv/tests/.tmpPjVbSH/python/3.11/python3 
-> /usr/bin/python3.11

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: python_list_duplicate_path_entries
Source: crates/uv/tests/it/python_list.rs:297
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-cpython-3.12.[X]-[PLATFORM]     [PYTHON-3.12]
          3 │+cpython-3.12.[X]-[PLATFORM]    [PYTHON-3.12]
    4     4 │ cpython-3.11.[X]-[PLATFORM]    [PYTHON-3.11]
    5     5 │ 
    6     6 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_list::python_list_duplicate_path_entries' panicked at /var/tmp/portage/dev-python/uv-0.6.14/work/cargo_home/gentoo/insta-1.42.2/src/runtime.rs:679:13:
snapshot assertion for 'python_list_duplicate_path_entries' failed in line 297
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
---- python_list::python_list stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
cpython-3.12.10-linux-x86_64-gnu    /var/tmp/portage/dev-python/uv-0.6.14/homedir/.local/share/uv/tests/.tmpfmR8dE/python/3.12/python3 -> /usr/lib/python-exec/python3.12/python3
cpython-3.11.12-linux-x86_64-gnu    /var/tmp/portage/dev-python/uv-0.6.14/homedir/.local/share/uv/tests/.tmpfmR8dE/python/3.11/python3 -> /usr/bin/python3.11

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: python_list-2
Source: crates/uv/tests/it/python_list.rs:21
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-cpython-3.12.[X]-[PLATFORM]     [PYTHON-3.12]
          3 │+cpython-3.12.[X]-[PLATFORM]    [PYTHON-3.12]
    4     4 │ cpython-3.11.[X]-[PLATFORM]    [PYTHON-3.11]
    5     5 │ 
    6     6 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_list::python_list' panicked at /var/tmp/portage/dev-python/uv-0.6.14/work/cargo_home/gentoo/insta-1.42.2/src/runtime.rs:
679:13:
snapshot assertion for 'python_list-2' failed in line 21
---- python_list::python_list_pin stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
Pinned `.python-version` to `3.12`

----- stderr -----

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
cpython-3.12.10-linux-x86_64-gnu    /var/tmp/portage/dev-python/uv-0.6.14/homedir/.local/share/uv/tests/.tmpxXP1zw/python/3.12/python3 -> /usr/lib/python-exec/python3.12/python3
cpython-3.11.12-linux-x86_64-gnu    /var/tmp/portage/dev-python/uv-0.6.14/homedir/.local/share/uv/tests/.tmpxXP1zw/python/3.11/python3 
-> /usr/bin/python3.11

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: python_list_pin-2
Source: crates/uv/tests/it/python_list.rs:145
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-cpython-3.12.[X]-[PLATFORM]     [PYTHON-3.12]
          3 │+cpython-3.12.[X]-[PLATFORM]    [PYTHON-3.12]
    4     4 │ cpython-3.11.[X]-[PLATFORM]    [PYTHON-3.11]
    5     5 │ 
    6     6 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_list::python_list_pin' panicked at /var/tmp/portage/dev-python/uv-0.6.14/work/cargo_home/gentoo/insta-1.42.2/src/runtime
.rs:679:13:
snapshot assertion for 'python_list_pin-2' failed in line 145

---- python_list::python_list_venv stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
cpython-3.12.10-linux-x86_64-gnu    /var/tmp/portage/dev-python/uv-0.6.14/homedir/.local/share/uv/tests/.tmpxQGsvc/python/3.12/python3 
-> /usr/lib/python-exec/python3.12/python3
cpython-3.11.12-linux-x86_64-gnu    /var/tmp/portage/dev-python/uv-0.6.14/homedir/.local/share/uv/tests/.tmpxQGsvc/python/3.11/python3 
-> /usr/bin/python3.11

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: python_list_venv-2
Source: crates/uv/tests/it/python_list.rs:186
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-cpython-3.12.[X]-[PLATFORM]     [PYTHON-3.12]
          3 │+cpython-3.12.[X]-[PLATFORM]    [PYTHON-3.12]
    4     4 │ cpython-3.11.[X]-[PLATFORM]    [PYTHON-3.11]
    5     5 │ 
    6     6 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_list::python_list_venv' panicked at /var/tmp/portage/dev-python/uv-0.6.14/work/cargo_home/gentoo/insta-1.42.2/src/runtime.rs:679:13:
snapshot assertion for 'python_list_venv-2' failed in line 186


failures:
    python_list::python_list
    python_list::python_list_duplicate_path_entries
    python_list::python_list_pin
    python_list::python_list_venv

test result: FAILED^O. 1804 passed; 4 failed; 3 ignored; 0 measured; 0 filtered out; finished in 457.40s
```

I guess tests were written with the assumption of 3.12.9, hence the extra space, and 3.12.10 broke the alignment expectations. Good news is, once updated for 3.12.10, they should be good going forward :-).

### Platform

Gentoo Linux amd64

### Version

0.6.14

### Python version

3.12.10, 3.11.12

---

_Label `bug` added by @mgorny on 2025-04-10 03:00_

---

_Comment by @zanieb on 2025-04-10 03:48_

Thanks! We should find a way to pad the spacing there, I guess, since the snapshot is intended to be patch version agnostic. 

---

_Label `testing` added by @zanieb on 2025-04-10 03:49_

---

_Renamed from "python_list tests fail with Pyton 3.12.10" to "python_list tests fail with Python 3.12.10" by @mgorny on 2025-04-10 09:00_

---

_Closed by @charliermarsh on 2025-04-14 12:23_

---

_Closed by @charliermarsh on 2025-04-14 12:23_

---
