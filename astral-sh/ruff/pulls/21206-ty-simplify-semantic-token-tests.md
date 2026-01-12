```yaml
number: 21206
title: "[ty] Simplify semantic token tests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/simplify-token-tests
created_at: 2025-11-02T15:53:25Z
updated_at: 2025-11-03T15:35:44Z
url: https://github.com/astral-sh/ruff/pull/21206
synced_at: 2026-01-12T15:57:18Z
```

# [ty] Simplify semantic token tests

---

_@MichaReiser_

## Summary

Our existing semantic tests were based on `CursorTest` which requires a `<CURSOR>` marker
in every test file. However, unlike most other LSP operations, `semantic_tokens` doesn't require a cursor position. Instead, the editor sends the range to highlight as part of the request (e.g. to only highlight the tokens currently in view)

This PR refactors the `semantic_tokens` test to use their own test infrastructure that 
doesn't require a `<CURSOR>` marker. 

I introduced a new `TestDb::init_program` helper to reduce the boilerplate
to initalize a new test db. 

I also used this opportunity to remove some unnecessary trailing whitespace
from some semantic token tests (which some editors remove automatically, 
which breaks the tests in turn). This results in some updated text ranges but 
doesn't change the tests.

## Test Plan

`cargo test`


---

_Label `testing` added by @MichaReiser on 2025-11-02 15:53_

---

_Label `ty` added by @MichaReiser on 2025-11-02 15:53_

---

_Review requested from @carljm by @MichaReiser on 2025-11-02 15:53_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-02 15:53_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-02 15:53_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-02 15:53_

---

_Label `testing` added by @MichaReiser on 2025-11-02 15:53_

---

_Label `ty` added by @MichaReiser on 2025-11-02 15:53_

---

_Comment by @github-actions[bot] on 2025-11-02 15:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-02 15:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @dcreager removed by @MichaReiser on 2025-11-02 15:59_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-02 15:59_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-02 15:59_

---

_@sharkdp approved on 2025-11-03 15:32_

Thank you!

---

_Merged by @MichaReiser on 2025-11-03 15:35_

---

_Closed by @MichaReiser on 2025-11-03 15:35_

---

_Branch deleted on 2025-11-03 15:35_

---
