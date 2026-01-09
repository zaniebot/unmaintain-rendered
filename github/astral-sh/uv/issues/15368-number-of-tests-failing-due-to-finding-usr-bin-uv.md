---
number: 15368
title: "Number of tests failing due to finding `/usr/bin/uv`"
type: issue
state: closed
author: mgorny
labels:
  - bug
  - testing
assignees: []
created_at: 2025-08-19T02:15:37Z
updated_at: 2025-09-02T13:45:15Z
url: https://github.com/astral-sh/uv/issues/15368
synced_at: 2026-01-07T13:12:19-06:00
---

# Number of tests failing due to finding `/usr/bin/uv`

---

_Issue opened by @mgorny on 2025-08-19 02:15_

### Summary

On top of uv 0.8.12, I'm seeing the following test failures (compared to 0.8.6 that was the last good version):

```
---- python_module::find_uv_bin_error_message stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 1 package in 7ms
Prepared 1 package in 53ms
Installed 1 package in 39ms
 + uv==0.1.0 (from file:///var/tmp/portage/dev-python/uv-0.8.12/work/uv-0.8.12/scripts/packages/fake-uv)

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
/usr/bin/uv

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: find_uv_bin_error_message-2
Source: crates/uv/tests/it/python_module.rs:443
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0       │-success: false
    1       │-exit_code: 1
          0 │+success: true
          1 │+exit_code: 0
    2     2 │ ----- stdout -----
          3 │+[PYTHON-BIN-3.12]/uv
    3     4 │ 
    4       │------ stderr -----
    5       │-Traceback (most recent call last):
    6       │-  File "<string>", line 1, in <module>
    7       │-  File "[SITE_PACKAGES]/uv/_find_uv.py", line 50, in find_uv_bin
    8       │-    raise UvNotFound(
    9       │-uv._find_uv.UvNotFound: Could not find the uv binary in any of the following locations:
   10       │- - [VENV]/[BIN]
   11       │- - [PYTHON-BIN-3.12]/
   12       │- - [SITE_PACKAGES]/[BIN]
   13       │- - [USER_SCHEME]/[BIN]
          5 │+----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_module::find_uv_bin_error_message' panicked at /var/tmp/portage/dev-python/uv-0.8.12/work/cargo_home/gentoo/insta-1.43.1
/src/runtime.rs:679:13:
snapshot assertion for 'find_uv_bin_error_message-2' failed in line 443
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- python_module::find_uv_bin_prefix stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.12.11 interpreter at: .venv/bin/python3
Resolved 1 package in 5ms
Prepared 1 package in 89ms
Installed 1 package in 31ms
 + uv==0.1.0 (from file:///var/tmp/portage/dev-python/uv-0.8.12/work/uv-0.8.12/scripts/packages/fake-uv)

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
/usr/bin/uv

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: find_uv_bin_prefix-2
Source: crates/uv/tests/it/python_module.rs:138
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-[TEMP_DIR]/prefix/[BIN]/uv
          3 │+/usr/[BIN]/uv
    4     4 │ 
    5     5 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_module::find_uv_bin_prefix' panicked at /var/tmp/portage/dev-python/uv-0.8.12/work/cargo_home/gentoo/insta-1.43.1/src/ru
ntime.rs:679:13:
snapshot assertion for 'find_uv_bin_prefix-2' failed in line 138

---- python_module::find_uv_bin_py38 stdout ----

thread 'python_module::find_uv_bin_py38' panicked at crates/uv/tests/it/common/mod.rs:1564:17:
Could not find Python 3.8 for test

---- python_module::find_uv_bin_in_ephemeral_environment stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
/usr/bin/uv

----- stderr -----
Resolved 1 package in 94ms
Audited in 0.06ms
Resolved 1 package in 6ms
Prepared 1 package in 68ms
Installed 1 package in 26ms
 + uv==0.1.0 (from file:///var/tmp/portage/dev-python/uv-0.8.12/work/uv-0.8.12/scripts/packages/fake-uv)

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: find_uv_bin_in_ephemeral_environment
Source: crates/uv/tests/it/python_module.rs:229
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-[CACHE_DIR]/archive-v0/[HASH]/[BIN]/uv
          3 │+/usr/[BIN]/uv
    4     4 │ 
    5     5 │ ----- stderr -----
    6     6 │ Resolved 1 package in [TIME]
    7     7 │ Audited in [TIME]
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_module::find_uv_bin_in_ephemeral_environment' panicked at /var/tmp/portage/dev-python/uv-0.8.12/work/cargo_home/gentoo/i
nsta-1.43.1/src/runtime.rs:679:13:
snapshot assertion for 'find_uv_bin_in_ephemeral_environment' failed in line 229

---- python_module::find_uv_bin_target stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Using CPython 3.12.11 interpreter at: .venv/bin/python3
Resolved 1 package in 8ms
Prepared 1 package in 65ms
Installed 1 package in 48ms
 + uv==0.1.0 (from file:///var/tmp/portage/dev-python/uv-0.8.12/work/uv-0.8.12/scripts/packages/fake-uv)

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
/usr/bin/uv

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: find_uv_bin_target-2
Source: crates/uv/tests/it/python_module.rs:92
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-[TEMP_DIR]/target/[BIN]/uv
          3 │+/usr/[BIN]/uv
    4     4 │ 
    5     5 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_module::find_uv_bin_target' panicked at /var/tmp/portage/dev-python/uv-0.8.12/work/cargo_home/gentoo/insta-1.43.1/src/ru
ntime.rs:679:13:
snapshot assertion for 'find_uv_bin_target-2' failed in line 92

---- python_module::find_uv_bin_in_parent_of_ephemeral_environment stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
/usr/bin/uv

----- stderr -----
Resolved 2 packages in 89ms
Prepared 1 package in 62ms
Installed 1 package in 33ms
 + uv==0.1.0 (from file:///var/tmp/portage/dev-python/uv-0.8.12/work/uv-0.8.12/scripts/packages/fake-uv)
Resolved 3 packages in 327ms
Prepared 3 packages in 233ms
Installed 3 packages in 44ms
 + anyio==4.3.0
 + idna==3.6
 + sniffio==1.3.1

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: find_uv_bin_in_parent_of_ephemeral_environment
Source: crates/uv/tests/it/python_module.rs:281
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-[VENV]/[BIN]/uv
          3 │+/usr/[BIN]/uv
    4     4 │ 
    5     5 │ ----- stderr -----
    6     6 │ Resolved 2 packages in [TIME]
    7     7 │ Prepared 1 package in [TIME]
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_module::find_uv_bin_in_parent_of_ephemeral_environment' panicked at /var/tmp/portage/dev-python/uv-0.8.12/work/cargo_hom
e/gentoo/insta-1.43.1/src/runtime.rs:679:13:
snapshot assertion for 'find_uv_bin_in_parent_of_ephemeral_environment' failed in line 281

---- python_module::find_uv_bin_user_bin stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Resolved 1 package in 6ms
Prepared 1 package in 56ms
Installed 1 package in 67ms
 + uv==0.1.0 (from file:///var/tmp/portage/dev-python/uv-0.8.12/work/uv-0.8.12/scripts/packages/fake-uv)

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
/var/tmp/portage/dev-python/uv-0.8.12/temp/uv/tests/.tmpHwzRLw/temp/.venv/bin/uv

----- stderr -----

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
/usr/bin/uv

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: find_uv_bin_user_bin-3
Source: crates/uv/tests/it/python_module.rs:373
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-[USER_SCHEME]/[BIN]/uv
          3 │+/usr/[BIN]/uv
    4     4 │ 
    5     5 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'python_module::find_uv_bin_user_bin' panicked at /var/tmp/portage/dev-python/uv-0.8.12/work/cargo_home/gentoo/insta-1.43.1/src/
runtime.rs:679:13:
snapshot assertion for 'find_uv_bin_user_bin-3' failed in line 373


failures:
    python_module::find_uv_bin_error_message
    python_module::find_uv_bin_in_ephemeral_environment
    python_module::find_uv_bin_in_parent_of_ephemeral_environment
    python_module::find_uv_bin_prefix
    python_module::find_uv_bin_py38
    python_module::find_uv_bin_target
    python_module::find_uv_bin_user_bin

test result: FAILED^O. 2131 passed; 7 failed; 2 ignored; 0 measured; 0 filtered out; finished in 471.37s
```

This is with `--features git --features pypi --features python --no-default-features`.

### Platform

Gentoo Linux amd64

### Version

0.8.12

### Python version

_No response_

---

_Label `bug` added by @mgorny on 2025-08-19 02:15_

---

_Label `testing` added by @zanieb on 2025-08-19 03:10_

---

_Comment by @zanieb on 2025-08-19 03:11_

Hm. I'm not sure if we have a great way to isolate these. We'd need to have Python report the wrong user bin directory I guess?

---

_Referenced in [astral-sh/uv#15379](../../astral-sh/uv/pulls/15379.md) on 2025-08-19 12:54_

---

_Comment by @mgorny on 2025-08-23 05:28_

I see a lot of paths there could evaluate to actual system/user directories where some `uv` is installed:

https://github.com/astral-sh/uv/blob/f1647838aed0c8be85b064e0407449ef220330cb/python/uv/_find_uv.py#L16-L36

But yeah, I suppose overriding some of the system vars to fake path prior to calling the function should suffice. Do you want me to give it a shot?

---

_Comment by @zanieb on 2025-08-23 14:39_

What would that look like, concretely?

---

_Comment by @mgorny on 2025-08-23 14:44_

I'm guessing some random bashing of `sys.prefix = "/dev/null"`, etc.

---

_Comment by @mgorny on 2025-08-29 04:37_

Gentle ping. Should I try with this idea?

---

_Referenced in [astral-sh/uv#15611](../../astral-sh/uv/pulls/15611.md) on 2025-08-31 19:11_

---

_Closed by @zanieb on 2025-09-02 13:45_

---
