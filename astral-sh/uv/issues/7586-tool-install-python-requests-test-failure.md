```yaml
number: 7586
title: tool_install_python_requests test failure
type: issue
state: closed
author: mgorny
labels:
  - bug
  - testing
assignees: []
created_at: 2024-09-20T14:32:15Z
updated_at: 2024-09-21T05:25:29Z
url: https://github.com/astral-sh/uv/issues/7586
synced_at: 2026-01-12T15:59:15Z
```

# tool_install_python_requests test failure

---

_@mgorny_

On top of 8259600ca6a5d278f3aa7825f5346a8dc7a851c3, Gentoo Linux amd64:

```
$ cargo test --test=tool_install tool_install_python_requests 
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.25s
     Running tests/tool_install.rs (target/debug/deps/tool_install-7a5b1e963b5fdf36)

running 1 test
test tool_install_python_requests ... FAILED

failures:

---- tool_install_python_requests stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: tool_install_python_requests-3
Source: crates/uv/tests/tool_install.rs:2126
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3     3 │ 
    4     4 │ ----- stderr -----
    5       │-Ignoring existing environment for `black`: the requested Python interpreter does not match the environment interpreter
    6       │-Resolved [N] packages in [TIME]
    7       │-Prepared [N] packages in [TIME]
    8       │-Installed [N] packages in [TIME]
    9       │- + black==24.3.0
   10       │- + click==8.1.7
   11       │- + mypy-extensions==1.0.0
   12       │- + packaging==24.0
   13       │- + pathspec==0.12.1
   14       │- + platformdirs==4.2.0
   15       │-Installed 2 executables: black, blackd
          5 │+`black` is already installed
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'tool_install_python_requests' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.40.0/src/runtime.rs:548:9:
snapshot assertion for 'tool_install_python_requests-3' failed in line 2126
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    tool_install_python_requests

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 32 filtered out; finished in 1.77s

error: test failed, to rerun pass `-p uv --test tool_install`
```

After hacking a fair bit to get some debug output, I've got:

```
          5 │+DEBUG uv [VERSION] ([COMMIT] DATE)
          6 │+DEBUG Searching for Python 3.11 in system path
          7 │+DEBUG Found `cpython-3.11.[X]-linux-x86_64-gnu` at `[PYTHON-3.11]` (search path)
          8 │+DEBUG Acquired lock for `tools`
          9 │+DEBUG Found existing environment for tool `black`: tools/black
         10 │+`black` is already installed
         11 │+DEBUG Released lock at `[TEMP_DIR]/tools/.lock`
```

Though it clearly did use Python 3.12 earlier on:

```
          5 │+DEBUG uv [VERSION] ([COMMIT] DATE)
          6 │+DEBUG Searching for Python 3.12 in system path
          7 │+DEBUG Found `cpython-3.11.[X]-linux-x86_64-gnu` at `[PYTHON-3.11]` (search path)
          8 │+DEBUG Found `cpython-3.12.[X]-linux-x86_64-gnu` at `[PYTHON-3.12]` (search path)
          9 │+DEBUG Acquired lock for `tools`
         10 │+DEBUG Using request timeout of [TIME]
         11 │+DEBUG Solving with installed Python version: 3.12.[X]
         12 │+DEBUG Solving with target Python version: >=3.12.[X]
         13 │+DEBUG Adding direct dependency: black*
         14 │+DEBUG No cache entry for: https://pypi.org/simple/black/
         15 │+DEBUG Searching for a compatible version of black (*)
         16 │+DEBUG Selecting: black==24.3.0 [compatible] (black-24.3.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
```

---

_Label `testing` added by @zanieb on 2024-09-20 16:35_

---

_Comment by @zanieb on 2024-09-20 16:37_

Thanks for the report! We can look into this. Did this regress after #7451?

---

_Comment by @zanieb on 2024-09-20 16:44_

Do your two interpreter versions have the same `sys.base_prefix`?

---

_Comment by @mgorny on 2024-09-20 16:58_

> Thanks for the report! We can look into this. Did this regress after #7451?

Yes.

> Do your two interpreter versions have the same `sys.base_prefix`?

Yes, all have `/usr`.

> @mgorny sorry for the troubles. If you are already tweaking that test, can you try adding a:
> 
> ```
>         .arg("--python-preference")
>         .arg("only-managed")
> ```
> 
> to the arguments and see if it passes again?

No problem. Yes, that makes the test pass.

---

_Comment by @mgorny on 2024-09-20 16:59_

Just to be clear, I've added it to the ultimate call, i.e.:

```diff
diff --git a/crates/uv/tests/tool_install.rs b/crates/uv/tests/tool_install.rs
index d045e651..3a0c59f4 100644
--- a/crates/uv/tests/tool_install.rs
+++ b/crates/uv/tests/tool_install.rs
@@ -2126,6 +2126,8 @@ fn tool_install_python_requests() {
     uv_snapshot!(context.filters(), context.tool_install()
         .arg("-p")
         .arg("3.11")
+        .arg("--python-preference")
+        .arg("only-managed")
         .arg("black")
         .env("UV_TOOL_DIR", tool_dir.as_os_str())
         .env("XDG_BIN_HOME", bin_dir.as_os_str())
```

---

_Comment by @zanieb on 2024-09-20 17:10_

Note that will fetch an interpreter — which I presume you don't want.

We need to improve our heuristic for whether or not a Python interpreter matches another. Right now we just compare the base prefix, but we probably need to compare the canonicalized sys.executable on Unix (we can't do that on Windows because they're always separate binaries).


---

_Label `bug` added by @zanieb on 2024-09-20 17:10_

---

_Assigned to @zanieb by @zanieb on 2024-09-20 17:10_

---

_Comment by @mgorny on 2024-09-20 17:25_

Perhaps `sysconfig.get_path("stdlib")` would work — it should be unique to every Python version installed to the same prefix, and definitely different for every prefix.

---

_Comment by @zanieb on 2024-09-20 17:29_

Thanks, maybe we'll use that

---

_Comment by @zanieb on 2024-09-20 18:11_

Thanks again for the report, this was a user-facing bug.

---

_Closed by @zanieb on 2024-09-20 19:01_

---

_Closed by @zanieb on 2024-09-20 19:01_

---

_Comment by @mgorny on 2024-09-21 05:25_

Thanks a lot!

---
