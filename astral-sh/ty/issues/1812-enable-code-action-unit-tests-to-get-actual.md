---
number: 1812
title: Enable code-action unit-tests to get actual diagnostics
type: issue
state: closed
author: Gankra
labels:
  - internal
assignees: []
created_at: 2025-12-08T22:37:42Z
updated_at: 2025-12-31T15:40:52Z
url: https://github.com/astral-sh/ty/issues/1812
synced_at: 2026-01-10T01:51:14Z
---

# Enable code-action unit-tests to get actual diagnostics

---

_Issue opened by @Gankra on 2025-12-08 22:37_

### Summary

Currently many of the code-action tests are e2e-only because they require getting "real" diagnostics and I wasn't sure how to do that outside of e2e tests. e2e tests are unpleasant and I should figure out a better solution.

### Version

_No response_

---

_Label `internal` added by @Gankra on 2025-12-08 22:38_

---

_Comment by @MichaReiser on 2025-12-08 22:42_

I added some code action tests that work well enough if all you need to match on is the diagnostic id

---

_Comment by @Gankra on 2025-12-08 22:49_

The auto-import stuff relies on also having the diagnostic_range, which is technically computed/passed by the server request handler. It's not too unreasonable to just assume it, but it would be nice to not have to. Maybe I'm overthinking it.

---

_Comment by @MichaReiser on 2025-12-08 23:03_

The tests also support a primary range

---

_Closed by @MichaReiser on 2025-12-31 15:40_

---
