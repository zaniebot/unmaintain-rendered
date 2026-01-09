---
number: 14114
title: "`test lock::lock_unique_named_index` times out on Windows (has been running for over 60 seconds)"
type: issue
state: open
author: Gankra
labels:
  - ci-flake
assignees: []
created_at: 2025-06-17T18:05:27Z
updated_at: 2025-06-26T17:27:07Z
url: https://github.com/astral-sh/uv/issues/14114
synced_at: 2026-01-07T13:12:18-06:00
---

# `test lock::lock_unique_named_index` times out on Windows (has been running for over 60 seconds)

---

_Issue opened by @Gankra on 2025-06-17 18:05_

https://github.com/astral-sh/uv/actions/runs/15714392091/job/44280406590?pr=14113

---

_Label `ci-flake` added by @Gankra on 2025-06-17 18:05_

---

_Renamed from "`test lock::lock_unique_named_index has been running for over 60 seconds`" to "`test lock::lock_unique_named_index` times out on Windows (has been running for over 60 seconds)" by @zanieb on 2025-06-17 18:10_

---

_Referenced in [astral-sh/uv#14158](../../astral-sh/uv/issues/14158.md) on 2025-06-20 20:08_

---

_Comment by @konstin on 2025-06-23 10:05_

Happens on macOS too: https://github.com/astral-sh/uv/actions/runs/15814501552/job/44570836722?pr=14205

---

_Comment by @oconnor663 on 2025-06-26 17:26_

I just got this failure running the full test suite _locally_ on Linux:

```
        FAIL [  52.917s] uv::it lock::lock_unique_named_index
  stdout ───

    running 1 test
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: lock_unique_named_index
    Source: crates/uv/tests/it/lock.rs:16354
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
        3     3 │
        4     4 │ ----- stderr -----
        5       │-Resolved 2 packages in [TIME]
              5 │+error: Request failed after 1 retries
              6 │+  Caused by: Failed to fetch: `https://example.com/iniconfig/`
              7 │+  Caused by: HTTP status client error (404 Not Found) for url (https://example.com/iniconfig/)
    ────────────┴───────────────────────────────────────────────────────────────────
    To update snapshots run `cargo insta review`
    Stopped on the first failure. Run `cargo insta test` to run all snapshots.
    test lock::lock_unique_named_index ... FAILED

    failures:

    failures:
        lock::lock_unique_named_index

    test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 2049 filtered out; finished in 52.91s

  stderr ───

    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Unfiltered output ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ----- stdout -----

    ----- stderr -----
    error: Request failed after 1 retries
      Caused by: Failed to fetch: `https://example.com/iniconfig/`
      Caused by: HTTP status client error (404 Not Found) for url (https://example.com/iniconfig/)

    ────────────────────────────────────────────────────────────────────────────────


    thread 'lock::lock_unique_named_index' panicked at /home/jacko/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/insta-1.43.1/src/runtime.rs:679:13:
    snapshot assertion for 'lock_unique_named_index' failed in line 16354
    note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---
