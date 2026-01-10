---
number: 14785
title: Since 0.8.0, version_get_fallback_unmanaged_json fails outside of a git checkout
type: issue
state: closed
author: musicinmybrain
labels:
  - bug
assignees: []
created_at: 2025-07-21T11:05:03Z
updated_at: 2025-07-21T14:17:07Z
url: https://github.com/astral-sh/uv/issues/14785
synced_at: 2026-01-10T01:25:48Z
---

# Since 0.8.0, version_get_fallback_unmanaged_json fails outside of a git checkout

---

_Issue opened by @musicinmybrain on 2025-07-21 11:05_

### Summary

When I run tests for 0.8.0 in a git checkout, everything is fine:

```
$ git clone https://github.com/astral-sh/uv
$ cd uv
$ git checkout 0.8.0
$ cargo run python install
$ cargo test
```

Otherwise, `version::version_get_fallback_unmanaged_json` fails:

```
$ curl -L -O https://github.com/astral-sh/uv/archive/0.8.0/uv-0.8.0.tar.gz
$ tar -xzf uv-0.8.0.tar.gz
$ cd uv-0.8.0
$ cargo run python install
$ cargo test
[…]

failures:

---- version::version_get_fallback_unmanaged_json stdout ----

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
----- stdout -----

----- stderr -----
error: The project is marked as unmanaged: `/tmp/uv/tests/.tmpV5Tavj/temp`

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: version_get_fallback_unmanaged_json
Source: crates/uv/tests/it/version.rs:1597
──────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
──────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬─────────────────────────────────────────────────────────────────────────────────────────
    0       │-success: true
    1       │-exit_code: 0
          0 │+success: false
          1 │+exit_code: 2
    2     2 │ ----- stdout -----
    3       │-{
    4       │-  "package_name": "uv",
    5       │-  "version": "[VERSION]",
    6       │-  "commit_info": null
    7       │-}
    8     3 │
    9     4 │ ----- stderr -----
   10       │-warning: Failed to read project metadata (The project is marked as unmanaged: `[TEMP_DIR]/`). Running `uv self version` for compatibility. This fallback will be removed in the future; pass `--preview` to force an error.
          5 │+error: The project is marked as unmanaged: `[TEMP_DIR]/`
────────────┴─────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'version::version_get_fallback_unmanaged_json' panicked at /home/ben/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/insta-1.43.1/src/runtime.rs:679:13:
snapshot assertion for 'version_get_fallback_unmanaged_json' failed in line 1597
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    version::version_get_fallback_unmanaged_json

test result: FAILED. 2111 passed; 1 failed; 3 ignored; 0 measured; 0 filtered out; finished in 158.22s

error: test failed, to rerun pass `-p uv --test it`
```

A similar but not identical problem occurred in the past, https://github.com/astral-sh/uv/issues/13212.

### Platform

Fedora 42 x86_64

### Version

uv 0.8.0

### Python version

Python 3.13.5

---

_Label `bug` added by @musicinmybrain on 2025-07-21 11:05_

---

_Comment by @zanieb on 2025-07-21 12:27_

I guess the logic in `git_version_info_expected` is wrong. Regardless, we removed this fallback in 0.8.0, so we should probably just remove the test.

---

_Assigned to @Copilot by @zanieb on 2025-07-21 12:27_

---

_Referenced in [astral-sh/uv#14786](../../astral-sh/uv/pulls/14786.md) on 2025-07-21 12:27_

---

_Closed by @zanieb on 2025-07-21 14:17_

---
