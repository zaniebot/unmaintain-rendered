---
number: 2672
title: "Flaky test `compile_invalid_pyc_invalidation_mode`"
type: issue
state: closed
author: zanieb
labels:
  - internal
  - testing
assignees: []
created_at: 2024-03-26T17:30:44Z
updated_at: 2024-07-07T20:21:07Z
url: https://github.com/astral-sh/uv/issues/2672
synced_at: 2026-01-10T01:23:20Z
---

# Flaky test `compile_invalid_pyc_invalidation_mode`

---

_Issue opened by @zanieb on 2024-03-26 17:30_

I see this relatively frequently on macOS

```
Snapshot: compile_invalid_pyc_invalidation_mode
Source: crates/uv/tests/pip_sync.rs:2934
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    7     7 │ Installed 1 package in [TIME]
    8     8 │ error: Failed to bytecode-compile Python file in: [SITE-PACKAGES]
    9     9 │   Caused by: Python process stderr:
   10    10 │ Invalid value for PYC_INVALIDATION_MODE "bogus", valid are "TIMESTAMP", "CHECKED_HASH", "UNCHECKED_HASH":
   11       │-  Caused by: Bytecode compilation failed, expected "[SITE-PACKAGES]/[FIRST-FILE]", received: ""
         11 │+  Caused by: Failed to write to Python stdin
         12 │+  Caused by: Broken pipe (os error 32)
```

---

_Label `internal` added by @zanieb on 2024-03-26 17:30_

---

_Label `testing` added by @zanieb on 2024-03-26 17:30_

---

_Comment by @charliermarsh on 2024-03-26 17:31_

Yeah same.

---

_Comment by @charliermarsh on 2024-03-26 21:03_

(This has also failed on CI FWIW.)

---

_Referenced in [astral-sh/uv#3465](../../astral-sh/uv/pulls/3465.md) on 2024-05-08 18:17_

---

_Comment by @zanieb on 2024-06-04 18:09_

@BurntSushi is reporting this on Linux now

---

_Referenced in [astral-sh/uv#4043](../../astral-sh/uv/pulls/4043.md) on 2024-06-05 11:17_

---

_Comment by @konstin on 2024-06-05 11:18_

I've been also been seeing this on linux rarely, no idea how to prevent this from happening, i made a PR to just retry once on failure

---

_Closed by @konstin on 2024-06-05 18:37_

---

_Comment by @zanieb on 2024-07-07 20:21_

We removed this test in #4863 — we should figure out how to test this some day.

---
