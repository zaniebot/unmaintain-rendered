```yaml
number: 19766
title: "Use fixed hash width for `ty_server` diagnostics"
type: pull_request
state: merged
author: ntBre
labels:
  - testing
assignees: []
merged: true
base: main
head: brent/hash-fix
created_at: 2025-08-05T14:43:11Z
updated_at: 2025-08-05T14:55:20Z
url: https://github.com/astral-sh/ruff/pull/19766
synced_at: 2026-01-12T15:56:46Z
```

# Use fixed hash width for `ty_server` diagnostics

---

_@ntBre_

Summary
--

Fixes a snapshot test failure I saw in #19653 locally and in Windows CI by
padding the hex ID to 16 digits to match the regex in `filter_result_id`.

https://github.com/astral-sh/ruff/blob/78e5fe0a51fcbb1456d54c65b4632f60c268e389/crates/ty_server/tests/e2e/pull_diagnostics.rs#L380-L384

Test Plan
--

I applied this to the branch from #19653 locally and saw that the tests now
pass. I couldn't reproduce this failure directly on `main` or this branch,
though.


---

_Review requested from @carljm by @ntBre on 2025-08-05 14:43_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-05 14:43_

---

_Review requested from @sharkdp by @ntBre on 2025-08-05 14:43_

---

_Review requested from @dcreager by @ntBre on 2025-08-05 14:43_

---

_Review request for @dcreager removed by @ntBre on 2025-08-05 14:43_

---

_Review request for @carljm removed by @ntBre on 2025-08-05 14:43_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-05 14:43_

---

_Label `testing` added by @ntBre on 2025-08-05 14:44_

---

_Comment by @github-actions[bot] on 2025-08-05 14:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-05 14:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @ntBre on 2025-08-05 14:55_

---

_Closed by @ntBre on 2025-08-05 14:55_

---

_Branch deleted on 2025-08-05 14:55_

---
