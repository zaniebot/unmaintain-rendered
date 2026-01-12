```yaml
number: 17024
title: "[syntax-errors] Single starred assignment target"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/single-starred-assignment-target
created_at: 2025-03-27T20:55:45Z
updated_at: 2025-03-29T16:35:49Z
url: https://github.com/astral-sh/ruff/pull/17024
synced_at: 2026-01-12T15:56:00Z
```

# [syntax-errors] Single starred assignment target

---

_@ntBre_

Summary
--

Detects starred assignment targets outside of tuples and lists like `*a = (1,)`.

This PR only considers assignment statements. I also checked annotated assigment statements, but these give a separate error that we already catch, so I think they're okay not to consider:

```pycon
>>> *a: list[int] = []
  File "<python-input-72>", line 1
    *a: list[int] = []
      ^
SyntaxError: invalid syntax
```

Fixes #13759

Test Plan
--

New inline tests, plus a new `SemanticSyntaxError` for an existing parser test. I also removed a now-invalid case from an otherwise-valid test fixture.

The new semantic error leads to two errors for the case below:

```python
*foo() = 42
```

but this matches [pyright] too.

[pyright]: https://pyright-play.net/?code=FQMw9mAUCUAEC8sAsAmAUEA

---

_Label `rule` added by @ntBre on 2025-03-27 20:55_

---

_Label `preview` added by @ntBre on 2025-03-27 20:55_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-27 20:55_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-27 20:55_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:84 on 2025-03-27 21:18_

I think I'd find it useful to keep the `test_err` closer to the `add_error` calls, so I'd switch the `test_err` and `test_ok` comments around and move them right above the `Self::add_error` call:
```rs
if let ... {
    // test_ok ...

    // test_err ...
	Self::add_error(...)
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:563 on 2025-03-27 21:22_

I got a bit confused as to why there are multiple branches for the same variants 😅

I might be missing something, please correct me if I'm wrong but I'd suggest to move the preview check condition inside the case block to avoid repeating the variants:
```rs
SemanticSyntaxErrorKind::ReboundComprehensionVariable
| SemanticSyntaxErrorKind::DuplicateTypeParameter
| SemanticSyntaxErrorKind::MultipleCaseAssignment(_)
| SemanticSyntaxErrorKind::IrrefutableCasePattern(_)
| SemanticSyntaxErrorKind::SingleStarredAssignment => {
	if self.settings.preview.is_enabled() {
		self.semantic_errors.borrow_mut().push(error);
	}
}
```

---

_@dhruvmanila approved on 2025-03-27 21:22_

Looks good apart from a couple minor comments!

---

_@ntBre reviewed on 2025-03-27 21:27_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:563 on 2025-03-27 21:27_

I think this was a clippy suggestion when there was only one other variant, but it's definitely getting annoying to update them (and to read them!)

---

_Comment by @github-actions[bot] on 2025-03-27 21:42_

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

_@MichaReiser approved on 2025-03-28 15:53_

---

_Merged by @ntBre on 2025-03-29 16:35_

---

_Closed by @ntBre on 2025-03-29 16:35_

---

_Branch deleted on 2025-03-29 16:35_

---
