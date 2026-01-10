```yaml
number: 21348
title: "[`refurb`] Detect empty f-strings (`FURB105`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-21346
created_at: 2025-11-09T20:25:55Z
updated_at: 2025-11-10T17:51:41Z
url: https://github.com/astral-sh/ruff/pull/21348
synced_at: 2026-01-10T16:53:55Z
```

# [`refurb`] Detect empty f-strings (`FURB105`)

---

_Pull request opened by @danparizher on 2025-11-09 20:25_

## Summary

Fixes FURB105 (`print-empty-string`) to detect empty f-strings in addition to regular empty strings. Previously, the rule only flagged `print("")` but missed `print(f"")`. This fix ensures both cases are detected and can be automatically fixed.

Fixes #21346

## Problem Analysis

The FURB105 rule checks for unnecessary empty strings passed to `print()` calls. The `is_empty_string` helper function was only checking for `Expr::StringLiteral` with empty values, but did not handle `Expr::FString` (f-strings). As a result, `print(f"")` was not being flagged as a violation, even though it's semantically equivalent to `print("")` and should be simplified to `print()`.

The issue occurred because the function used a `matches!` macro that only checked for string literals:

```rust
fn is_empty_string(expr: &Expr) -> bool {
    matches!(
        expr,
        Expr::StringLiteral(ast::ExprStringLiteral { value, .. }) if value.is_empty()
    )
}
```

## Approach

1. **Import the helper function**: Added `is_empty_f_string` to the imports from `ruff_python_ast::helpers`, which already provides logic to detect empty f-strings.

2. **Update `is_empty_string` function**: Changed the implementation from a `matches!` macro to a `match` expression that handles both string literals and f-strings:

   ```rust
   fn is_empty_string(expr: &Expr) -> bool {
       match expr {
           Expr::StringLiteral(ast::ExprStringLiteral { value, .. }) => value.is_empty(),
           Expr::FString(f_string) => is_empty_f_string(f_string),
           _ => false,
       }
   }
   ```

The fix leverages the existing `is_empty_f_string` helper function which properly handles the complexity of f-strings, including nested f-strings and interpolated expressions. This ensures the detection is accurate and consistent with how empty strings are detected elsewhere in the codebase.

---

_@ntBre approved on 2025-11-10 17:21_

Thank you!

I'm just going to close and reopen to try to trigger the ecosystem comment.

---

_Closed by @ntBre on 2025-11-10 17:21_

---

_Reopened by @ntBre on 2025-11-10 17:21_

---

_Label `rule` added by @ntBre on 2025-11-10 17:21_

---

_Comment by @ntBre on 2025-11-10 17:41_

Still no comment, but I downloaded the artifact:

> ### Linter (stable)
> ✅ ecosystem check detected no linter changes.
> 
> ### Linter (preview)
> ✅ ecosystem check detected no linter changes.



---

_Merged by @ntBre on 2025-11-10 17:41_

---

_Closed by @ntBre on 2025-11-10 17:41_

---

_Branch deleted on 2025-11-10 17:51_

---
