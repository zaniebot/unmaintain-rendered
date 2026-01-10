```yaml
number: 15317
title: "Regression: panic messages about invalid mdtests are now swallowed by mdtest"
type: issue
state: closed
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
created_at: 2025-01-07T14:14:14Z
updated_at: 2025-01-08T16:34:52Z
url: https://github.com/astral-sh/ruff/issues/15317
synced_at: 2026-01-10T11:09:56Z
```

# Regression: panic messages about invalid mdtests are now swallowed by mdtest

---

_Issue opened by @AlexWaygood on 2025-01-07 14:14_

Following #15241, some of the internal error messages from `red_knot_test` are now swallowed by the framework, for example this one here:

https://github.com/astral-sh/ruff/blob/b26448926a8c2f61befa17b8ffa1dd0ac3232c9c/crates/red_knot_test/src/lib.rs#L32-L37

On a PR branch I was working on, I got this (not very informative!) test failure:

![image](https://github.com/user-attachments/assets/b918b880-9ea9-41a7-9829-3e8b38a7c8cf)

If I revert https://github.com/astral-sh/ruff/commit/75015b0ed91b728f22897c3bf12af5e6451e163a on that PR branch, I get this instead, which is much more helpful:

![image](https://github.com/user-attachments/assets/37d85078-dbba-42b3-a83d-7099b8e4cb4b)

---

_Label `testing` added by @AlexWaygood on 2025-01-07 14:14_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-07 14:14_

---

_Label `help wanted` added by @AlexWaygood on 2025-01-07 14:14_

---

_Assigned to @dcreager by @dcreager on 2025-01-07 14:20_

---

_Label `help wanted` removed by @AlexWaygood on 2025-01-07 14:21_

---

_Comment by @dcreager on 2025-01-07 14:26_

The panic that is being swallowed is not within the closure that is wrapped in `catch_unwind`.  I think the issue here is that the panic hook is global, and so if you're running tests concurrently, one thread might install the custom panic hook for its `catch_unwind` call while another thread is in the middle of e.g. this parse operation that might fail.

---

_Closed by @dcreager on 2025-01-08 16:34_

---
