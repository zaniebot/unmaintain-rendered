---
number: 4374
title: cache_prune tests fail when run from unpacked sources
type: issue
state: closed
author: mgorny
labels: []
assignees: []
created_at: 2024-06-18T04:25:57Z
updated_at: 2024-06-18T07:15:41Z
url: https://github.com/astral-sh/uv/issues/4374
synced_at: 2026-01-07T13:12:17-06:00
---

# cache_prune tests fail when run from unpacked sources

---

_Issue opened by @mgorny on 2024-06-18 04:25_

When building uv and running the test suite from unpacked sources (e.g. https://github.com/astral-sh/uv/archive/refs/tags/0.2.12.tar.gz), the following tests fail:

```
     Running tests/cache_prune.rs (target/debug/deps/cache_prune-a20668bb0c4fb0dd)

running 3 tests
test prune_no_op ... FAILED
test prune_stale_directory ... FAILED
test prune_stale_symlink ... FAILED

failures:

---- prune_no_op stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: prune_no_op
Source: crates/uv/tests/cache_prune.rs:77
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
    5       │-DEBUG uv [VERSION] ([COMMIT] DATE)
          5 │+DEBUG uv 0.2.12
    6     6 │ Pruning cache at: [CACHE_DIR]/
    7     7 │ No unused entries found
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'prune_no_op' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.39.0/src/runtime.rs:562:9:
snapshot assertion for 'prune_no_op' failed in line 77

---- prune_stale_directory stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: prune_stale_directory
Source: crates/uv/tests/cache_prune.rs:115
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
    5       │-DEBUG uv [VERSION] ([COMMIT] DATE)
          5 │+DEBUG uv 0.2.12
    6     6 │ Pruning cache at: [CACHE_DIR]/
    7     7 │ DEBUG Removing dangling cache entry: [CACHE_DIR]/simple-v4
    8     8 │ Removed 1 directory
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'prune_stale_directory' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.39.0/src/runtime.rs:562:9:
snapshot assertion for 'prune_stale_directory' failed in line 115
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- prune_stale_symlink stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: prune_stale_symlink
Source: crates/uv/tests/cache_prune.rs:161
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
    5       │-DEBUG uv [VERSION] ([COMMIT] DATE)
          5 │+DEBUG uv 0.2.12
    6     6 │ Pruning cache at: [CACHE_DIR]/
    7     7 │ DEBUG Removing dangling cache entry: [CACHE_DIR]/archive-v0/[ENTRY]
    8     8 │ Removed 44 files ([SIZE])
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'prune_stale_symlink' panicked at /home/mgorny/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.39.0/src/runtime.rs:562:9:
snapshot assertion for 'prune_stale_symlink' failed in line 161


failures:
    prune_no_op
    prune_stale_directory
    prune_stale_symlink

test result: FAILED. 0 passed; 3 failed; 0 ignored; 0 measured; 0 filtered out; finished in 1.21s
```

I'm going to submit a pull request shortly.

---

_Referenced in [astral-sh/uv#4375](../../astral-sh/uv/pulls/4375.md) on 2024-06-18 04:54_

---

_Closed by @charliermarsh on 2024-06-18 07:15_

---
