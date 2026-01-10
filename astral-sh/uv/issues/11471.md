```yaml
number: 11471
title: "`run::detect_infinite_recursion` failure (needs `python-managed` mark?)"
type: issue
state: closed
author: mgorny
labels:
  - bug
  - testing
assignees: []
created_at: 2025-02-13T07:08:40Z
updated_at: 2025-02-17T01:05:02Z
url: https://github.com/astral-sh/uv/issues/11471
synced_at: 2026-01-10T03:50:31Z
```

# `run::detect_infinite_recursion` failure (needs `python-managed` mark?)

---

_Issue opened by @mgorny on 2025-02-13 07:08_

### Summary

This time filing a bug instead of guessing the fix :-).

```
failures:

---- run::detect_infinite_recursion stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
error: No interpreter found for Python 3.13.0 in virtual environments, managed installations, or search path

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: detect_infinite_recursion
Source: crates/uv/tests/it/run.rs:4230
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    1     1 │ exit_code: 2
    2     2 │ ----- stdout -----
    3     3 │ 
    4     4 │ ----- stderr -----
    5       │-error: `uv run` was recursively invoked 6 times which exceeds the limit of 5.
    6       │-
    7       │-hint: If you are running a script with `uv run` in the shebang, you may need to include the `--script` flag.
          5 │+error: No interpreter found for Python 3.13.0 in virtual environments, managed installations, or search path
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'run::detect_infinite_recursion' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.42.1/src/runtime.rs:679:13:
snapshot assertion for 'detect_infinite_recursion' failed in line 4230
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    run::detect_infinite_recursion

test result: FAILED. 1661 passed; 1 failed; 3 ignored; 0 measured; 0 filtered out; finished in 300.10s
```

Curious enough, in our test environment it failed slightly differently:

```
failures:

---- run::detect_infinite_recursion stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
Downloading cpython-3.13.0-linux-x86_64-gnu (18.1MiB)
 Downloaded cpython-3.13.0-linux-x86_64-gnu
error: `uv run` was recursively invoked 6 times which exceeds the limit of 5.

hint: If you are running a script with `uv run` in the shebang, you may need to include the `--script` flag.

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: detect_infinite_recursion
Source: crates/uv/tests/it/run.rs:4230
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
          5 │+Downloading cpython-3.13.0-linux-x86_64-gnu ([SIZE])
          6 │+ Downloaded cpython-3.13.0-linux-x86_64-gnu
    5     7 │ error: `uv run` was recursively invoked 6 times which exceeds the limit of 5.
    6     8 │ 
    7     9 │ hint: If you are running a script with `uv run` in the shebang, you may need to include the `--script` flag.
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'run::detect_infinite_recursion' panicked at /var/tmp/portage/dev-python/uv-0.5.31/work/cargo_home/gentoo/insta-1.42.1/src/runtime.rs:679:13:
snapshot assertion for 'detect_infinite_recursion' failed in line 4230
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    run::detect_infinite_recursion

test result: FAILED^O. 1659 passed; 1 failed; 3 ignored; 0 measured; 0 filtered out; finished in 600.62s
```

### Platform

Gentoo Linux amd64

### Version

0.5.31

### Python version

Python 3.12.9

---

_Label `bug` added by @mgorny on 2025-02-13 07:08_

---

_Comment by @mgorny on 2025-02-13 07:09_

(both systems have Python 3.13.2 installed too, but `python` points to 3.12)

---

_Comment by @zanieb on 2025-02-13 15:21_

This just sounds like a bug, let me look into it.

---

_Label `testing` added by @zanieb on 2025-02-13 15:21_

---

_Comment by @zanieb on 2025-02-13 15:31_

Turning on debug logs, you can see this searches for Python instead of using the test context because it's missing all of our standard test command flags

```
    DEBUG uv [VERSION] ([COMMIT] DATE)
    DEBUG Found project root: `[WORKSPACE]/`
    DEBUG Project `uv` is marked as unmanaged
    DEBUG No project found; searching for Python interpreter
    DEBUG Reading Python requests from version file at `[WORKSPACE]/.python-versions`
    DEBUG Searching for Python 3.13.0 in virtual environments, managed installations, or search path
```

---

_Closed by @zanieb on 2025-02-13 16:59_

---

_Comment by @twardoch on 2025-02-17 01:05_

> hint: If you are running a script with `uv run` in the shebang, you may need to include the `--script` flag.

But include WHERE. Make the hint clear. 

---
