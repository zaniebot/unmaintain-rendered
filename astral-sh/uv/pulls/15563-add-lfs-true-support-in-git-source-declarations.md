```yaml
number: 15563
title: "Add `lfs = true` support in Git source declarations"
type: pull_request
state: closed
author: zanieb
labels:
  - "test:macos"
assignees: []
draft: true
base: main
head: zb/lfs-true
created_at: 2025-08-27T20:13:30Z
updated_at: 2025-12-13T15:44:07Z
url: https://github.com/astral-sh/uv/pull/15563
synced_at: 2026-01-12T16:11:49Z
```

# Add `lfs = true` support in Git source declarations

---

_@zanieb_

Incomplete draft for https://github.com/astral-sh/uv/issues/13485

---

_Comment by @zanieb on 2025-08-27 20:56_

It looks like LFS is working in CI!

```

    running 1 test
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: sync_git_lfs-2
    Source: crates/uv/tests/it/sync.rs:13407
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
        3     3 │ 
        4       │------ stderr -----
        5       │-Traceback (most recent call last):
        6       │-  File "<string>", line 1, in <module>
        7       │-    import test_lfs_repo.lfs_module
        8       │-  File "[SITE_PACKAGES]/test_lfs_repo/lfs_module.py", line 1
        9       │-    version https://git-lfs.github.com/spec/v1
       10       │-            ^^^^^
       11       │-SyntaxError: invalid syntax
              4 │+----- stderr -----
    ────────────┴───────────────────────────────────────────────────────────────────
    Stopped on the first failure. Run `cargo insta test` to run all snapshots.
    test sync::sync_git_lfs ... FAILED
```

So... my problem is it's not working on my machine.

---

_Label `test:macos` added by @zanieb on 2025-08-27 20:56_

---

_Comment by @zanieb on 2025-12-13 15:44_

Finished in #16143

---

_Closed by @zanieb on 2025-12-13 15:44_

---
