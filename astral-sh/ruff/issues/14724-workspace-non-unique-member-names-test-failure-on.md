```yaml
number: 14724
title: "`workspace_non_unique_member_names` test failure on 32-bit platforms"
type: issue
state: closed
author: fossdd
labels:
  - testing
  - ty
assignees: []
created_at: 2024-12-02T06:43:26Z
updated_at: 2024-12-02T08:42:21Z
url: https://github.com/astral-sh/ruff/issues/14724
synced_at: 2026-01-12T15:54:54Z
```

# `workspace_non_unique_member_names` test failure on 32-bit platforms

---

_@fossdd_

The following test failure was experienced only on 32-bit platforms (`armhf`, `armv7`, `aarch64`):

```
---- workspace::metadata::tests::workspace_non_unique_member_names stdout ----
thread 'workspace::metadata::tests::workspace_non_unique_member_names' panicked at crates/red_knot_workspace/src/workspace/metadata.rs:617:9:
assertion `left == right` failed
  left: "the workspace contains two packages named 'a': '/app/packages/b' and '/app/packages/a'"
 right: "the workspace contains two packages named 'a': '/app/packages/a' and '/app/packages/b'"
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
failures:
    workspace::metadata::tests::workspace_non_unique_member_names
test result: FAILED. 12 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.07s
error: test failed, to rerun pass `-p red_knot_workspace --lib`
```

Thanks!

---

_Label `testing` added by @MichaReiser on 2024-12-02 07:22_

---

_Closed by @MichaReiser on 2024-12-02 07:36_

---

_Comment by @fossdd on 2024-12-02 07:40_

Thank you for the fast fix!

---

_Label `red-knot` added by @AlexWaygood on 2024-12-02 08:42_

---
