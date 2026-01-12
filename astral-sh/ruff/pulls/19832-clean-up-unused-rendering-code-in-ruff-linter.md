```yaml
number: 19832
title: "Clean up unused rendering code in `ruff_linter`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/cleanup-unused-code
created_at: 2025-08-08T17:49:26Z
updated_at: 2025-08-09T18:20:50Z
url: https://github.com/astral-sh/ruff/pull/19832
synced_at: 2026-01-12T15:56:48Z
```

# Clean up unused rendering code in `ruff_linter`

---

_@ntBre_

## Summary

This is a follow-up to https://github.com/astral-sh/ruff/pull/19415#discussion_r2263456740 to remove some unused code. As Micha noticed, `GroupedEmitter::with_show_source` was only used in local unit tests[^1] and was safe to remove. This allowed deleting `MessageCodeFrame` and a lot more helper code previously shared with the `full` output format.

I also moved some other code from `text.rs` and `message/mod.rs` into `grouped.rs` that is now only used for the `grouped` format. With a little refactoring of the `concise` rendering logic in `ruff_db`, we could probably remove `RuleCodeAndBody` too. The only difference I see from the `concise` output is whether we print the filename next to the row and column or not:

```shell
> ruff check --output-format concise
try.py:1:8: F401 [*] `math` imported but unused
> ruff check --output-format grouped
try.py:
  1:8 F401 [*] `math` imported but unused
```

But I didn't try to do that here.

## Test Plan

Existing tests, with the source code no longer displayed. I also deleted one test, as it was now a duplicate of the `default` test.

[^1]: "Local unit tests" as opposed to all of our linter snapshot tests, as is the case for `TextEmitter::with_show_fix_diff`. We also want to expose that to users eventually (https://github.com/astral-sh/ruff/issues/7352), which I don't believe is the case for the `grouped` format.


---

_Comment by @github-actions[bot] on 2025-08-08 17:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Brent/cleanup unused code" to "Clean up unused rendering code in `ruff_linter`" by @ntBre on 2025-08-08 18:44_

---

_Label `internal` added by @ntBre on 2025-08-08 18:44_

---

_Comment by @github-actions[bot] on 2025-08-08 18:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 18:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @ntBre on 2025-08-08 18:57_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-08 18:57_

---

_@MichaReiser approved on 2025-08-09 16:33_

Nice

---

_Merged by @ntBre on 2025-08-09 18:20_

---

_Closed by @ntBre on 2025-08-09 18:20_

---

_Branch deleted on 2025-08-09 18:20_

---
