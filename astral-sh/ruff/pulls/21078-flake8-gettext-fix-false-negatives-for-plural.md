```yaml
number: 21078
title: "[`flake8-gettext`] Fix false negatives for plural argument of ngettext (`INT001`, `INT002`, `INT003`)"
type: pull_request
state: open
author: danparizher
labels:
  - rule
  - preview
assignees: []
base: main
head: fix-21062
created_at: 2025-10-26T01:31:37Z
updated_at: 2025-12-29T22:03:22Z
url: https://github.com/astral-sh/ruff/pull/21078
synced_at: 2026-01-10T16:36:18Z
```

# [`flake8-gettext`] Fix false negatives for plural argument of ngettext (`INT001`, `INT002`, `INT003`)

---

_Pull request opened by @danparizher on 2025-10-26 01:31_

## Summary
Fixes #21062. Extends the `INT001`, `INT002`, and `INT003` lint rules to check both the singular and plural arguments of `ngettext` function calls, fixing false negatives where formatting issues in the plural argument were not being detected.

## Problem Analysis
The three rules (`f-string-in-get-text-func-call`, `format-in-get-text-func-call`, and `printf-in-get-text-func-call`) only checked the first argument (singular) of `ngettext` calls but ignored the second argument (plural). This caused false negatives when f-strings, `.format()` calls, or `%` formatting were used in the plural argument.

The root cause was:
1. Rule functions only examined `args.first()` 
2. No logic to determine if a function call was specifically `ngettext` vs `gettext`
3. Missing checks for `args.get(1)` (the second argument)

Example of the bug:
```python
ngettext(f"one item", f"{n} items", n)  # Only flagged first f-string, not second
```

## Approach
1. **Modified function signatures**: Updated all three rule functions to accept the `func` parameter
2. **Added ngettext detection**: Created `is_ngettext_call()` helper that checks:
   - Direct name references (e.g., `ngettext(...)`)
   - Qualified names ending with `ngettext` (e.g., `gettext_mod.ngettext(...)`)
3. **Extended argument checking**: For `ngettext` calls, now check both first and second arguments
4. **Code refactoring**: Created shared `helpers.rs` module to eliminate duplication
5. **Updated main checker**: Modified expression analyzer to pass `func` parameter


---

_Comment by @github-actions[bot] on 2025-10-26 01:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_gettext/snapshots/ruff_linter__rules__flake8_gettext__tests__preview__printf-in-get-text-func-call_INT003.py.snap`:200 on 2025-12-29 21:53_

I think we have to customize the error messages depending on whether it's the first argument or the second. The `consider ...` suggestion doesn't make much sense here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_gettext/helpers.rs`:5 on 2025-12-29 21:54_

We have an existing helper in `flake8_gettext/mod.rs`. I kind of think we should keep them together, either by moving this there or moving the other helper here. I don't have a strong preference but would slightly lean toward reusing the existing file.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_gettext/helpers.rs`:18 on 2025-12-29 22:00_

We should double check if these two are redundant. I would expect `resolve_qualified_name` on a name to return `["ngettext"]`, which should pass the second check here, but I could be wrong. Checking the name might be a small perf optimization since the qualified name can be expensive, but I'd still be pretty tempted to turn the whole function into

```rust
checker.semantic().resolve_qualified_name(func).is_some_and(|qualified_name|
    matches!(qualified_name, [.., "ngettext"])
)
```

if it works.

---

_@ntBre requested changes on 2025-12-29 22:02_

Thank you! And sorry for the delay on the review.

I only had a couple of nits about the code, but I think we need to improve the error messages for these new cases. We should also make this a preview change as I mentioned on the issue since this substantially increases the scope of the rule, even if there are no ecosystem changes.

---

_Label `rule` added by @ntBre on 2025-12-29 22:03_

---

_Label `preview` added by @ntBre on 2025-12-29 22:03_

---
