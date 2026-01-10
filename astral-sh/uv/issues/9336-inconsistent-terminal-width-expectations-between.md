---
number: 9336
title: "Inconsistent terminal width expectations between `built_by_uv_editable-2` and help tests"
type: issue
state: closed
author: mgorny
labels:
  - testing
assignees: []
created_at: 2024-11-21T20:34:17Z
updated_at: 2024-11-26T10:07:54Z
url: https://github.com/astral-sh/uv/issues/9336
synced_at: 2026-01-10T01:24:39Z
---

# Inconsistent terminal width expectations between `built_by_uv_editable-2` and help tests

---

_Issue opened by @mgorny on 2024-11-21 20:34_

Hit it with 0.5.4 and a513301b7a0a5d1dd840868ccfb82f49cbdbcd48.

There's a bunch of help tests in uv that assume a 100 character wide terminal. To make them pass in the past, I've started using `COLUMNS=100` to run the test suite. However, the most recent release introduced `built_by_uv_editable-2` that actually assumes a 80-character wide terminal. Hence, whichever `COLUMNS` value I use, some tests fail.

```console
$ COLUMNS=80 cargo test --test it --no-default-features
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.23s
     Running tests/it/main.rs (target/debug/deps/it-a93b0d5fb81fa362)

running 108 tests
[…]
   53    61 │           Allow insecure connections to a host [env: UV_INSECURE_HOST=]
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   57    65 │           Change to the given directory prior to running the command
   58    66 │       --project <PROJECT>
   59    67 │           Run the command within the given project directory
   60    68 │       --config-file <CONFIG_FILE>
   61       │-          The path to a `uv.toml` file to use for configuration [env: UV_CONFIG_FILE=]
         69 │+          The path to a `uv.toml` file to use for configuration [env:
         70 │+          UV_CONFIG_FILE=]
   62    71 │       --no-config
   63       │-          Avoid discovering configuration files (`pyproject.toml`, `uv.toml`) [env: UV_NO_CONFIG=]
         72 │+          Avoid discovering configuration files (`pyproject.toml`, `uv.toml`)
         73 │+          [env: UV_NO_CONFIG=]
   64    74 │   -h, --help
   65    75 │           Display the concise help for this command
   66    76 │   -V, --version
   67    77 │           Display the uv version
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'help::help_with_no_pager' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.41.1/src/runtime.rs:680:13:
snapshot assertion for 'help_with_no_pager' failed in line 999


failures:
    help::help
    help::help_flag
    help::help_flag_subcommand
    help::help_flag_subsubcommand
    help::help_short_flag
    help::help_subcommand
    help::help_subsubcommand
    help::help_with_global_option
    help::help_with_no_pager

test result: FAILED. 99 passed; 9 failed; 0 ignored; 0 measured; 0 filtered out; finished in 22.53s

error: test failed, to rerun pass `-p uv --test it`
$ COLUMNS=100 cargo test --test it --no-default-features -F python
   Compiling uv v0.5.4 (/home/mgorny/git/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 42.46s
     Running tests/it/main.rs (target/debug/deps/it-bed166d36b75cf3e)

running 147 tests
[…]
failures:

---- build_backend::built_by_uv_editable stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
built_by_uv-0.1.0-py3-none-any.whl

----- stderr -----

────────────────────────────────────────────────────────────────────────────────


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----
..                                                                                           [100%]
2 passed in 0.01s

----- stderr -----

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: built_by_uv_editable-2
Source: crates/uv/tests/it/build_backend.rs:189
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-..                                                                       [100%]
          3 │+..                                                                                           [100%]
    4     4 │ 2 passed in [TIME]
    5     5 │ 
    6     6 │ ----- stderr -----
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'build_backend::built_by_uv_editable' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.41.1/src/runtime.rs:680:13:
snapshot assertion for 'built_by_uv_editable-2' failed in line 189
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    build_backend::built_by_uv_editable

test result: FAILED. 146 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 32.34s

error: test failed, to rerun pass `-p uv --test it`
```

My guess would be that uv uses terminal width since it's "outer", while pytest defaults to 80 since it's run with piped stdout. The former implies we need to use `COLUMNS` if we don't have exactly the same terminal width as CI, but `COLUMNS` leaks into pytest and modifies its output.

---

_Comment by @charliermarsh on 2024-11-21 22:34_

Sorry about that. We'll figure out something else.

---

_Label `testing` added by @charliermarsh on 2024-11-21 22:34_

---

_Comment by @charliermarsh on 2024-11-21 22:35_

\cc @konstin in case you have ideas

---

_Comment by @konstin on 2024-11-21 22:51_

We're capturing pytest's behavior here, not uv's. We can set `COLUMNS` for the pytest invocations, but it's probably better to filter out that line entirely and only keep the `2 passed in` line.

---

_Referenced in [astral-sh/uv#9436](../../astral-sh/uv/pulls/9436.md) on 2024-11-26 09:14_

---

_Closed by @konstin on 2024-11-26 09:59_

---

_Comment by @mgorny on 2024-11-26 10:07_

Thank you!

---

_Referenced in [astral-sh/uv#11588](../../astral-sh/uv/issues/11588.md) on 2025-02-18 07:14_

---
