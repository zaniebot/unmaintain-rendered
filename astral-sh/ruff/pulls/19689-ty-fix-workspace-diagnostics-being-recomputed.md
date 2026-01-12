```yaml
number: 19689
title: "[ty] Fix workspace diagnostics being recomputed"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/fix-url-mismatch
created_at: 2025-08-01T15:15:47Z
updated_at: 2025-08-04T12:17:18Z
url: https://github.com/astral-sh/ruff/pull/19689
synced_at: 2026-01-12T15:56:45Z
```

# [ty] Fix workspace diagnostics being recomputed

---

_@MichaReiser_

## Summary

This fixes a bug where the server invalidated all diagnostics from the previous workspace diagnostic request because
the url's sent by the client and the url's sent by the server didn't match up.

Fixes https://github.com/astral-sh/ty/issues/924

## Test Plan

Before



https://github.com/user-attachments/assets/f8419727-3fa9-48ed-a61f-f861858dfc90

With the fix applied

https://github.com/user-attachments/assets/df1f9659-2610-47c7-a5d0-7b1b70e058b9



---

_Label `bug` added by @MichaReiser on 2025-08-01 15:15_

---

_Label `server` added by @MichaReiser on 2025-08-01 15:15_

---

_Label `ty` added by @MichaReiser on 2025-08-01 15:15_

---

_Label `bug` added by @MichaReiser on 2025-08-01 15:15_

---

_Label `server` added by @MichaReiser on 2025-08-01 15:15_

---

_Label `ty` added by @MichaReiser on 2025-08-01 15:15_

---

_Comment by @github-actions[bot] on 2025-08-01 15:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-01 15:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-08-01 15:51_

---

_Review requested from @carljm by @MichaReiser on 2025-08-01 15:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-01 15:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-01 15:51_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-01 15:51_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-08-01 15:51_

---

_Review request for @dcreager removed by @MichaReiser on 2025-08-01 15:51_

---

_Review request for @carljm removed by @MichaReiser on 2025-08-01 15:51_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-08-01 15:51_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:308 on 2025-08-04 06:46_

If by any chance you've the non-exact conversion URL example handy, it might be useful to add it in the doc as an example to make it easier to understand as instance of when it isn't going to be the same.

---

_@dhruvmanila approved on 2025-08-04 06:46_

Thank you!

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:308 on 2025-08-04 06:47_

Or, we could add an integration test with that URL :)

---

_@dhruvmanila reviewed on 2025-08-04 06:47_

---

_@MichaReiser reviewed on 2025-08-04 11:35_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:308 on 2025-08-04 11:35_

Yes, that's a great idea (but it was a bit painful haha)

---

_Label `server` removed by @MichaReiser on 2025-08-04 11:47_

---

_Label `bug` removed by @MichaReiser on 2025-08-04 11:47_

---

_Label `internal` added by @MichaReiser on 2025-08-04 11:47_

---

_Merged by @MichaReiser on 2025-08-04 11:49_

---

_Closed by @MichaReiser on 2025-08-04 11:49_

---

_Branch deleted on 2025-08-04 11:49_

---

_@dhruvmanila reviewed on 2025-08-04 12:17_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:308 on 2025-08-04 12:17_

Thank you for adding the test!

---
