```yaml
number: 11030
title: Fix best-interpreter lookups when there is an invalid interpreter in the PATH
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-best-interp
created_at: 2025-01-28T18:30:49Z
updated_at: 2025-01-28T19:44:34Z
url: https://github.com/astral-sh/uv/pull/11030
synced_at: 2026-01-12T16:09:38Z
```

# Fix best-interpreter lookups when there is an invalid interpreter in the PATH

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/10978

The root cause is the same as #10908 — I should have been more careful with the original change.

---

_Label `bug` added by @zanieb on 2025-01-28 18:30_

---

_Review requested from @Gankra by @zanieb on 2025-01-28 18:31_

---

_Review requested from @charliermarsh by @zanieb on 2025-01-28 18:31_

---

_@charliermarsh approved on 2025-01-28 19:38_

What's the nested Result? What do the two levels indicate?

---

_Comment by @zanieb on 2025-01-28 19:43_

This was a design decision in Python discovery a while back (which I'm sort of regretting as it was the source of this bug)

The outer error is if we _fail_ during discovery for some reason.
The inner error is we don't find a Python interpreter.

https://github.com/astral-sh/uv/blob/db4ab9dc8aecd9c4deaffd6036304aae70d93791/crates/uv-python/src/discovery.rs#L166-L179

The bug is that I changed things so a deferred discovery failure is sometimes raised instead of the inner error to improve error messages when there's an invalid interpreter on the path. However, this broke some downstream code where we take actions after failing to find an interpreter and counted the function to only return _fatal_ outer errors. 

---

_Comment by @zanieb on 2025-01-28 19:44_

I was worried about breakage here when I made the change, but foolishly trusted the test coverage (which it seems didn't exist for the case of a broken interpreter)

---

_Merged by @zanieb on 2025-01-28 19:44_

---

_Closed by @zanieb on 2025-01-28 19:44_

---

_Branch deleted on 2025-01-28 19:44_

---
