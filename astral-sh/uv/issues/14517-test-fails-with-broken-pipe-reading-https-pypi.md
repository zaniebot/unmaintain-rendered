```yaml
number: 14517
title: "Test fails with broken pipe reading `https://pypi.org/simple/<package>`"
type: issue
state: open
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-07-09T12:11:20Z
updated_at: 2025-07-09T14:37:59Z
url: https://github.com/astral-sh/uv/issues/14517
synced_at: 2026-01-12T16:01:50Z
```

# Test fails with broken pipe reading `https://pypi.org/simple/<package>`

---

_@zanieb_

```
running 1 test
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: extra_inferences
    Source: crates/uv/tests/it/lock_conflict.rs:5205
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
        5       │-Resolved 137 packages in [TIME]
              5 │+error: Failed to fetch: `[https://pypi.org/simple/psutil/`](https://pypi.org/simple/psutil/%60)
              6 │+  Caused by: request or response body error
              7 │+  Caused by: error reading a body from connection
              8 │+  Caused by: stream closed because of a broken pipe
```

---

_Label `ci-flake` added by @zanieb on 2025-07-09 12:11_

---

_Comment by @charliermarsh on 2025-07-09 13:25_

What is going on with the link being in brackets, and the %60?

---

_Comment by @zanieb on 2025-07-09 14:37_

That's a good question, I have no idea.

---
