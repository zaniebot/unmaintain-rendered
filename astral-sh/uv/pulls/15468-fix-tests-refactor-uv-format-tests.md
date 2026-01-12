```yaml
number: 15468
title: "fix(tests): Refactor uv format tests"
type: pull_request
state: merged
author: pposca
labels:
  - internal
assignees: []
merged: true
base: main
head: fix/15458_refactor_uv_format_tests
created_at: 2025-08-23T09:57:04Z
updated_at: 2025-08-26T10:20:56Z
url: https://github.com/astral-sh/uv/pull/15468
synced_at: 2026-01-12T16:11:45Z
```

# fix(tests): Refactor uv format tests

---

_@pposca_

Closes #15458

## Summary

Refactor uv format tests to reduce noise.




---

_@zanieb reviewed on 2025-08-23 14:34_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:61 on 2025-08-23 14:34_

Should this be `assert_snapshot`? Shouldn't we just be asserting the string matches this exact content?

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:61 on 2025-08-23 14:35_

If we're moving these out into utilities that make assertions about it being formatted or not, it seems weird to use snapshots.

---

_@zanieb reviewed on 2025-08-23 14:35_

---

_Comment by @zanieb on 2025-08-23 14:37_

I'm a little worried that this diverges from the rest of the test suite, in which a `pyproject.toml` is always explicitly constructed and snapshots are generally used rather than assertions. I'm not sure if that's a big deal or not though.

I'd probably chase making the Python file much simpler first, e.g., `x     = 1`, so there's less noise.

---

_@pposca reviewed on 2025-08-23 19:24_

---

_Review comment by @pposca on `crates/uv/tests/it/format.rs`:61 on 2025-08-23 19:24_

My first idea was to use assert_eq, which would allow to use const &str instead of @"string literal" and the function, but I've never used test snapshots and I didn't know if I would break anything.

What is the role of the snapshots here? As far as I've understood, the common use requires running the test more than one to compare snapshots; but what it seems like is that they are being used like assert_eq in these tests.



---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:61 on 2025-08-23 19:28_

Snapshots work like string matching assertions that we can automatically update instead of failing when there's a mismatch. I don't even know what will happen if you share it across multiple tests like this, probably bad things in the long-term.

---

_@zanieb reviewed on 2025-08-23 19:28_

---

_Assigned to @zanieb by @konstin on 2025-08-25 10:36_

---

_@zanieb approved on 2025-08-25 12:40_

---

_Comment by @zanieb on 2025-08-25 12:40_

Thank you!

---

_Merged by @zanieb on 2025-08-25 12:40_

---

_Closed by @zanieb on 2025-08-25 12:40_

---

_Label `internal` added by @zanieb on 2025-08-25 12:40_

---

_Branch deleted on 2025-08-26 10:20_

---
