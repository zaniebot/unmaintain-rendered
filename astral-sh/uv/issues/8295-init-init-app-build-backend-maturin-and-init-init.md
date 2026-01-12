```yaml
number: 8295
title: "`init::init_app_build_backend_maturin` and `init::init_lib_build_backend_maturin` test failures (offline?)"
type: issue
state: closed
author: mgorny
labels: []
assignees: []
created_at: 2024-10-17T17:01:39Z
updated_at: 2024-10-18T16:39:10Z
url: https://github.com/astral-sh/uv/issues/8295
synced_at: 2026-01-12T15:59:23Z
```

# `init::init_app_build_backend_maturin` and `init::init_lib_build_backend_maturin` test failures (offline?)

---

_@mgorny_

I'm seeing the two following test failures in Gentoo with uv-0.4.23:

```
---- init::init_app_build_backend_maturin stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: init_app_build_backend_maturin-7
Source: crates/uv/tests/it/init.rs:2667
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0       │-success: true
    1       │-exit_code: 0
          0 │+success: false
          1 │+exit_code: 2
    2     2 │ ----- stdout -----
    3       │-Hello from foo!
    4     3 │ 
    5     4 │ ----- stderr -----
    6     5 │ Using CPython 3.12.[X] interpreter at: [PYTHON-3.12]
    7     6 │ Creating virtual environment at: .venv
    8     7 │ Resolved 1 package in [TIME]
    9       │-Prepared 1 package in [TIME]
   10       │-Installed 1 package in [TIME]
   11       │- + foo==0.1.0 (from file://[TEMP_DIR]/foo)
          8 │+error: Failed to prepare distributions
          9 │+  Caused by: Failed to build `foo @ file://[TEMP_DIR]/foo`
         10 │+  Caused by: Build backend failed to build editable through `build_editable` (exit status: 1)
         11 │+
         12 │+[stdout]
         13 │+Running `maturin pep517 build-wheel -i [CACHE_DIR]/builds-v0/[TMP]/python --compatibility off --editable`
         14 │+
         15 │+[stderr]
         16 │+error: no matching package found
         17 │+searched package name: `pyo3`
         18 │+perhaps you meant:      glob, itoa, log, ...
         19 │+location searched: registry `crates-io`
         20 │+required by package `foo v0.1.0 ([TEMP_DIR]/foo)`
         21 │+As a reminder, you're using offline mode (--offline) which can sometimes cause surprising resolution failures, if this error is too confusing you may wish to retry without the offline flag.
         22 │+💥 maturin failed
         23 │+  Caused by: Cargo metadata failed. Does your crate compile with `cargo build`?
         24 │+  Caused by: `cargo metadata` exited with an error: 
         25 │+Error: command ['maturin', 'pep517', 'build-wheel', '-i', '[CACHE_DIR]/builds-v0/[TMP]/python', '--compatibility', 'off', '--editable'] returned non-zero exit status 1
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'init::init_app_build_backend_maturin' panicked at /var/tmp/portage/dev-python/uv-0.4.23/work/cargo_home/gentoo/insta-1.40.0/src/runtime.rs:548:9:
snapshot assertion for 'init_app_build_backend_maturin-7' failed in line 2667
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- init::init_lib_build_backend_maturin stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: init_lib_build_backend_maturin-7
Source: crates/uv/tests/it/init.rs:2934
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0       │-success: true
    1       │-exit_code: 0
          0 │+success: false
          1 │+exit_code: 2
    2     2 │ ----- stdout -----
    3       │-Hello from foo!
    4     3 │ 
    5     4 │ ----- stderr -----
    6     5 │ Using CPython 3.12.[X] interpreter at: [PYTHON-3.12]
    7     6 │ Creating virtual environment at: .venv
    8     7 │ Resolved 1 package in [TIME]
    9       │-Prepared 1 package in [TIME]
   10       │-Installed 1 package in [TIME]
   11       │- + foo==0.1.0 (from file://[TEMP_DIR]/foo)
          8 │+error: Failed to prepare distributions
          9 │+  Caused by: Failed to build `foo @ file://[TEMP_DIR]/foo`
         10 │+  Caused by: Build backend failed to build editable through `build_editable` (exit status: 1)
         11 │+
         12 │+[stdout]
         13 │+Running `maturin pep517 build-wheel -i [CACHE_DIR]/builds-v0/[TMP]/python --compatibility off --editable`
         14 │+
         15 │+[stderr]
         16 │+error: no matching package found
         17 │+searched package name: `pyo3`
         18 │+perhaps you meant:      glob, itoa, log, ...
         19 │+location searched: registry `crates-io`
         20 │+required by package `foo v0.1.0 ([TEMP_DIR]/foo)`
         21 │+As a reminder, you're using offline mode (--offline) which can sometimes cause surprising resolution failures, if this error is too confusing you may wish to retry without the offline flag.
         22 │+💥 maturin failed
         23 │+  Caused by: Cargo metadata failed. Does your crate compile with `cargo build`?
         24 │+  Caused by: `cargo metadata` exited with an error: 
         25 │+Error: command ['maturin', 'pep517', 'build-wheel', '-i', '[CACHE_DIR]/builds-v0/[TMP]/python', '--compatibility', 'off', '--editable'] returned non-zero exit status 1
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'init::init_lib_build_backend_maturin' panicked at /var/tmp/portage/dev-python/uv-0.4.23/work/cargo_home/gentoo/insta-1.40.0/src/runtime.rs:548:9:
snapshot assertion for 'init_lib_build_backend_maturin-7' failed in line 2934
```

My educated guess would be that the test assumes that `cargo` will be able to download `pyo3` from crates.io, but we're running everything 100% offline here. Could you please add a marker/feature to skip these tests? Alternatively, I think having `pyo3` (along with its dependencies) in `Cargo.lock` would also work.

---

_Closed by @charliermarsh on 2024-10-18 16:30_

---

_Comment by @mgorny on 2024-10-18 16:39_

Thanks!

---
