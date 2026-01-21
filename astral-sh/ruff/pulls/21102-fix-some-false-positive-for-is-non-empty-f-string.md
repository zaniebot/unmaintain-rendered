```yaml
number: 21102
title: "Fix some false positive for `is_non_empty_f_string`"
type: pull_request
state: closed
author: IDrokin117
labels:
  - bug
assignees: []
base: main
head: feature/gh-21048
created_at: 2025-10-27T19:44:53Z
updated_at: 2026-01-21T10:29:42Z
url: https://github.com/astral-sh/ruff/pull/21102
synced_at: 2026-01-21T10:59:59Z
```

# Fix some false positive for `is_non_empty_f_string`

---

_@IDrokin117_

## Summary
Closes #21048

Fix some false positive edge cases when determining string truthiness. The following cases are now treated as unknown truthiness:

1. Comparison expressions can return any value, even empty strings.
2. F-strings with format specs can produce either empty or non-empty strings:
```python
bool(f"{'mystring':.0}") # Returns False
``` 

Additionally, fixed a bug that caused `f"{f""!s}"` to be treated as non-empty. Since `.iter().all()` returns true if the iterator is empty, f-strings with no elements (like nested `f"{f""!s}"`) were incorrectly treated as non-empty.

## Test Plan
- Added test cases to S604 since `Truthiness` has no tests of its own

---

_Comment by @github-actions[bot] on 2025-10-27 20:02_

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

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1405 on 2025-10-28 19:43_

Should this just be `any` instead? I could certainly be wrong, but it seems like we should check if any part is non-empty rather than `all` parts are non-empty. That would also give the correct behavior for the empty iterator and match the outer `any` on line 1401.

I tried this locally, and I think it fixed an existing bug in a SIM222 test too.

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1362 on 2025-10-28 19:43_

Should we move this down to the other complex expressions below? It seems slightly out of place in this block now that the other variants are all `true`.

---

_@ntBre reviewed on 2025-10-28 19:47_

Thanks! This looks good, I just had a couple of small ideas/suggestions.

---

_Label `bug` added by @ntBre on 2025-10-28 19:47_

---

_Review comment by @dscorbett on `crates/ruff_python_ast/src/helpers.rs`:1362 on 2025-10-28 20:06_

To be more precise, this could still be `true` if all the operators in the `Compare` are the ones guaranteed to return `bool`s (`in`, `is`, `is not`, and `not in`).

---

_@dscorbett reviewed on 2025-10-28 20:06_

---

_Comment by @codspeed-hq[bot] on 2025-10-29 14:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/IDrokin117%3Afeature%2Fgh-21048?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21102 will **not alter performance**

<sub>Comparing <code>IDrokin117:feature/gh-21048</code> (be1c7ab) with <code>main</code> (96b60c1)</sub>



### Summary

`✅ 52` untouched  





---

_@MichaReiser requested changes on 2025-10-30 14:00_

Thank. I'll mark this as requested changes to make it easier to track the state of this PR. Please request re-review once you addressed the changes from the inline comments.

---

_Closed by @MichaReiser on 2026-01-21 10:29_

---
