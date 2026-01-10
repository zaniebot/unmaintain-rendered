```yaml
number: 19136
title: "[`refurb`] `FURB164` fix should validate arguments and should usually be marked unsafe"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19076
created_at: 2025-07-04T03:12:59Z
updated_at: 2025-07-16T15:51:41Z
url: https://github.com/astral-sh/ruff/pull/19136
synced_at: 2026-01-10T17:58:13Z
```

# [`refurb`] `FURB164` fix should validate arguments and should usually be marked unsafe

---

_Pull request opened by @danparizher on 2025-07-04 03:12_

## Summary

Fixes #19076

An attempt at fixing #19076 where the rule could change program behavior by incorrectly converting from_float/from_decimal method calls to constructor calls.

The fix implements argument validation using Ruff's existing type inference system (`ResolvedPythonType`, `typing::is_int`, `typing::is_float`) to determine when conversions are actually safe, adds logic to detect invalid method calls (wrong argument counts, incorrect keyword names) and suppress fixes for them, and changes the default fix applicability from `Safe` to `Unsafe` with safe fixes only offered when the argument type is known to be compatible and no problematic keywords are used.

One uncertainty is whether the type inference catches all possible edge cases in complex codebases, but the new approach is significantly more conservative and safer than the previous implementation.

## Test Plan

I updated the existing test fixtures with edge cases from the issue and manually verified behavior with temporary test files for valid/unsafe/invalid scenarios.

---

_Comment by @github-actions[bot] on 2025-07-04 03:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-07-14 09:19_

---

_Label `bug` added by @ntBre on 2025-07-14 15:52_

---

_Label `fixes` added by @ntBre on 2025-07-14 15:52_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:80 on 2025-07-14 15:56_

nit: we tend to keep the main rule function at the top of the file, followed by any helpers.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:154 on 2025-07-14 16:07_

The nested `match`es look very similar in all three branches here. Could we possibly do the `resolve_type` match first and then the method and constructor match? It looks like we might need to save `is_int` and `is_float` variables to check later.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:187 on 2025-07-14 16:11_

I _think_ this could be simplified by checking `call.arguments.len()` which should account for both `args` and `keywords` and then using `find_argument` to locate it by name or position:


```suggestion
            // from_float(f) - should have exactly one positional argument or 'f' keyword
            if call.arguments.len() != 1 {
                false
            } else {
                call.arguments.find_argument("f", 0).is_some()
            }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:191 on 2025-07-14 16:12_

Same as above, if it works above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:187 on 2025-07-14 16:15_

We may also need to match on the constructor here. `Decimal.from_float` is positional-only, unless that's checked somewhere else:

```pycon
>>> from decimal import Decimal
>>> Decimal.from_float(f=123)
Traceback (most recent call last):
  File "<python-input-7>", line 1, in <module>
    Decimal.from_float(f=123)
    ~~~~~~~~~~~~~~~~~~^^^^^^^
TypeError: Decimal.from_float() takes no keyword arguments
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:250 on 2025-07-14 16:16_

We should just move this check after the other `report_diagnostic` call instead of duplicating the construction.

```suggestion
    // Validate that the method call has correct arguments
    if !has_valid_method_arguments(call, method_name) {
        // Don't suggest a fix for invalid calls
        return;
    }
```

This would have made more sense when we used to have to create a diagnostic and later push it to the `Checker`, but merely creating the diagnostic now will set it up to be pushed on `Drop` so we can just return after creating it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:264 on 2025-07-14 16:18_

It might make sense to return an `Option` from `has_valid_method_arguments` instead of finding the argument values again here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:340 on 2025-07-14 16:26_

nit: I'd probably put this right after the `let-else` checking that it's a `Call`. I'm not sure which of these checks is most expensive, but that makes sense logically at least.

---

_@ntBre reviewed on 2025-07-14 17:50_

Thanks, this looks good to me. I just had some suggestions for simplifying the code a bit.

---

_Review requested from @ntBre by @danparizher on 2025-07-14 20:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:185 on 2025-07-16 14:57_

```suggestion
        arg_expr
            .as_name_expr()
            .and_then(|name| semantic.only_binding(name).map(|id| semantic.binding(id)))
            .map(|binding| {
                (
                    typing::is_int(binding, semantic),
                    typing::is_float(binding, semantic),
                )
            })
            .unwrap_or_default()
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:223 on 2025-07-16 14:57_

```suggestion
                arg_expr
                    .as_call_expr()
                    .and_then(|call| semantic.resolve_qualified_name(&call.func))
                    .is_some_and(|qualified_name| {
                        matches!(qualified_name.segments(), ["decimal", "Decimal"])
                    })
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:249 on 2025-07-16 15:01_

```suggestion
                call.arguments.find_positional(0)
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:241 on 2025-07-16 15:03_

I see now that we could actually convert the `len` check to an early return since it's shared by both branches:


```suggestion
    if call.arguments.len() != 1 {
        return None;
    }
    
    match method_name {
        MethodName::FromFloat => {
            // Decimal.from_float is positional-only; Fraction.from_float allows keyword 'f'.
            if constructor == Constructor::Decimal {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:261 on 2025-07-16 15:03_

```suggestion
            call.arguments.find_argument_value("dec", 0)
```

after the early return above.

---

_@ntBre approved on 2025-07-16 15:05_

Thanks! Just a few more slight simplifications.

---

_Merged by @ntBre on 2025-07-16 15:38_

---

_Closed by @ntBre on 2025-07-16 15:38_

---

_Branch deleted on 2025-07-16 15:51_

---
