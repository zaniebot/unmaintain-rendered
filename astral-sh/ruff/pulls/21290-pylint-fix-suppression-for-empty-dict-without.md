```yaml
number: 21290
title: "[`pylint`] Fix suppression for empty dict without tuple key annotation (`PLE1141`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-21289
created_at: 2025-11-06T01:08:00Z
updated_at: 2025-11-21T23:38:29Z
url: https://github.com/astral-sh/ruff/pull/21290
synced_at: 2026-01-12T15:57:20Z
```

# [`pylint`] Fix suppression for empty dict without tuple key annotation (`PLE1141`)

---

_@danparizher_

## Summary

Fixes the PLE1141 (`dict-iter-missing-items`) rule to allow fixes for empty dictionaries unless they have type annotations indicating 2-tuple keys. Previously, the fix was incorrectly suppressed for all empty dicts due to vacuous truth in the `all()` function.

Fixes #21289

## Problem Analysis

The `is_dict_key_tuple_with_two_elements` function was designed to suppress the fix when a dictionary's keys are all 2-tuples, as unpacking tuple keys directly would change runtime behavior.

However, for empty dictionaries, `iter_keys()` returns an empty iterator, and `all()` on an empty iterator returns `true` (vacuous truth). This caused the function to incorrectly suppress fixes for empty dicts, even when there was no indication that future keys would be 2-tuples.

## Approach

1. **Detect empty dictionaries**: Added a check to identify when a dict literal has no keys.

2. **Handle annotated empty dicts**: For empty dicts with type annotations:
   - Parse the annotation to check if it's `dict[tuple[T1, T2], ...]` where the tuple has exactly 2 elements
   - Support both PEP 484 (`typing.Dict`, `typing.Tuple`) and PEP 585 (`dict`, `tuple`) syntax
   - If tuple keys are detected, suppress the fix (correct behavior)
   - Otherwise, allow the fix

3. **Handle unannotated empty dicts**: For empty dicts without annotations, allow the fix since there's no indication that keys will be 2-tuples.

4. **Preserve existing behavior**: For non-empty dicts, the original logic is unchanged - check if all existing keys are 2-tuples.

The implementation includes helper functions:
- `is_annotation_dict_with_tuple_keys()`: Checks if a type annotation specifies dict with tuple keys
- `is_tuple_type_with_two_elements()`: Checks if a type expression represents a 2-tuple

Test cases were added to verify:
- Empty dict without annotation triggers the error
- Empty dict with `dict[tuple[int, str], bool]` suppresses the error
- Empty dict with `dict[str, int]` triggers the error
- Existing tests remain unchanged

---

_Comment by @github-actions[bot] on 2025-11-06 01:18_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `rule` added by @ntBre on 2025-11-06 21:30_

---

_Label `preview` added by @ntBre on 2025-11-06 21:30_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/dict_iter_missing_items.py`:41 on 2025-11-06 21:35_

I think it would be interesting to have these two lines in separate test cases, i.e.

```py
empty_dict_annotated_tuple_keys: dict[tuple[int, str], bool] = {}
for k, v in empty_dict_annotated_tuple_keys:
    pass

empty_dict_annotated_tuple_keys = {}
empty_dict_annotated_tuple_keys[("x", "y")] = True
for k, v in empty_dict_annotated_tuple_keys:
    pass
```

I don't think we have to skip the second case even if it's technically incorrect because it's likely to be rare, as mentioned on the issue. But it seems useful to capture our behavior in this case.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:127 on 2025-11-11 14:03_

```suggestion
    let (value, annotation) = match statement {
        Stmt::Assign(assign_stmt) => {
            (assign_stmt.value, None)
        }
        Stmt::AnnAssign(ann_assign_stmt) => {
            let Some(value) = ann_assign_stmt.value.as_ref() else {
                return false;
            };
            (value, Some(ann_assign_stmt.annotation.as_ref()))
        }
        _ => return false,
```

It might even be nicer with something like:

```rust
        Stmt::AnnAssign(ast::StmtAnnAssign{ value: Some(value), annotation, .. }) => {
            (value, Some(annotation))
        }
```

and then we can keep/share the `Expr::Dict` check below instead of duplicating it in both match arms.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:131 on 2025-11-11 14:04_

`ExprDict` has an `is_empty` method, which I think would work here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:140 on 2025-11-11 14:07_

I think this is all just:

```suggestion
        return annotation.is_some_and(|annotation| is_annotation_dict_with_tuple_keys(annotation, semantic));
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:159 on 2025-11-11 14:08_

If we're not handling strings, which I think is fine, we can just omit this check since it's redundant with the one below.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:169 on 2025-11-11 14:10_

Should this just be a let-else as well?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:174 on 2025-11-11 14:10_

I think we should check that `dict` has its builtin binding with `match_builtin_expr`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:162 on 2025-11-11 14:12_

It might make sense to do something like:

```suggestion
    // dict[K, V] format - check if K is tuple with 2 elements
    if let [key, _value] = tuple.elts.as_slice() {
        return is_tuple_type_with_two_elements(key, semantic);
    }
```

just to make sure there's the expected number of elements.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:199 on 2025-11-11 14:13_

Again I think we need to check for builtin bindings to `tuple` to make sure it's not shadowed.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:202 on 2025-11-11 14:18_

I don't think this is a valid annotation, but I could be wrong. I think the ellipsis can only come after the first element. I would probably just check the `tuple.len()` here.

---

_@ntBre requested changes on 2025-11-11 14:21_

---

_Review requested from @ntBre by @danparizher on 2025-11-11 22:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:131 on 2025-11-19 20:17_

I may have gotten this backwards, sorry. Should it be `is_none_or` instead of `is_some_and`? I'm wondering because I was expecting a diagnostic at line 46 of the new tests, but we're not getting one.

---

_@ntBre reviewed on 2025-11-19 20:18_

Thanks, this is looking great! I just had one question about the expected result of the new test case.

---

_@danparizher reviewed on 2025-11-20 00:56_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:131 on 2025-11-20 00:56_

Thanks for catching that. The logic was correct (`is_some_and` is right), but I made it explicit for clarity.

For empty dicts:
- With annotation indicating tuple keys → suppress fix (return `true`)
- Without annotation → allow fix (return `false`)

The `is_some_and` chain was correct, but I refactored it to an explicit `if let Some` pattern to make the behavior clearer.

---

_Review requested from @ntBre by @danparizher on 2025-11-20 00:56_

---

_@ntBre reviewed on 2025-11-21 22:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:131 on 2025-11-21 22:02_

Oh I see, the two test cases were interfering because they both referred to the same annotation above. I think the `is_some_and` is clear then, I was mostly just confused about the end result. I'll put it back. Thanks for fixing the test!

---

_@ntBre approved on 2025-11-21 22:03_

Thanks!

---

_Merged by @ntBre on 2025-11-21 22:07_

---

_Closed by @ntBre on 2025-11-21 22:07_

---

_Branch deleted on 2025-11-21 23:38_

---
