```yaml
number: 14909
title: "[`flake8-comprehensions`] Skip `C416` if comprehension contains unpacking"
type: pull_request
state: merged
author: harupy
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-C416-unpack
created_at: 2024-12-11T12:33:23Z
updated_at: 2024-12-20T09:50:03Z
url: https://github.com/astral-sh/ruff/pull/14909
synced_at: 2026-01-12T15:55:49Z
```

# [`flake8-comprehensions`] Skip `C416` if comprehension contains unpacking

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix #11482. Applies https://github.com/adamchainz/flake8-comprehensions/pull/205 to ruff. `C416` should be skipped if comprehension contains unpacking. Here's an example:

```python
list_of_lists = [[1, 2], [3, 4]]

# ruff suggests `list(list_of_lists)` here, but that would change the result.
# `list(list_of_lists)` is not `[(1, 2), (3, 4)]`
a = [(x, y) for x, y in list_of_lists]

# This is equivalent to `list(list_of_lists)`
b = [x for x in list_of_lists]
```

## Test Plan

<!-- How was it tested? -->

Existing checks


---

_Comment by @github-actions[bot] on 2024-12-11 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_comprehensions/snapshots/ruff_linter__rules__flake8_comprehensions__tests__C416_C416.py.snap`:95 on 2024-12-11 12:41_

Once type inference is supported and `d` is inferred as a `dict`, C416 should flag this line.

---

_@harupy reviewed on 2024-12-11 12:41_

---

_@dylwil3 reviewed on 2024-12-11 12:57_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_comprehensions/snapshots/ruff_linter__rules__flake8_comprehensions__tests__C416_C416.py.snap`:95 on 2024-12-11 12:57_

We do have limited support for telling when something is a dictionary - at least if it is defined in the same file where the lint violation occurs. See: https://github.com/astral-sh/ruff/blob/881375a8d9b30291316af70b0e1e9327b7bb9ab7/crates/ruff_python_semantic/src/analyze/typing.rs#L836 .

You can see examples of how this is used in other rules, e.g. https://github.com/astral-sh/ruff/blob/881375a8d9b30291316af70b0e1e9327b7bb9ab7/crates/ruff_linter/src/rules/flake8_simplify/rules/key_in_dict.rs#L123-L135

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:141 on 2024-12-11 13:13_

Should I rewrite this to:

```rust
    let (
        Expr::Name(ast::ExprName {
            id: target_key_name,
            ..
        }),
        Expr::Name(ast::ExprName {
            id: target_value_name,
            ..
        }),
        Expr::Name(ast::ExprName { id: key_name, .. }),
        Expr::Name(ast::ExprName { id: value_name, .. }),
    ) = (&target_key, &target_value, &key, &value)
    else {
        return;
    };
```

?

---

_@harupy reviewed on 2024-12-11 13:13_

---

_Label `rule` added by @AlexWaygood on 2024-12-11 15:50_

---

_Label `bug` added by @AlexWaygood on 2024-12-11 15:51_

---

_@dhruvmanila reviewed on 2024-12-12 07:21_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:141 on 2024-12-12 07:21_

I think it's fine as is. You could possibly merge the `target_key_name` and `target_value_name` above like:

```rust
    let [Expr::Name(ast::ExprName { id: target_key, .. }), Expr::Name(ast::ExprName { id: target_value, .. })] = elts.as_slice() else {
        return;
    };
```

---

_Renamed from "Skip `C416` if comprehension contains unpacking" to "[`flake8-comprehensions`] Skip `C416` if comprehension contains unpacking" by @dylwil3 on 2024-12-13 04:24_

---

_Review requested from @dhruvmanila by @harupy on 2024-12-17 13:49_

---

_Review requested from @dylwil3 by @harupy on 2024-12-17 13:49_

---

_Comment by @harupy on 2024-12-17 13:49_

@dylwil3 @dhruvmanila Thanks for the comments. Could you take another look?

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_comprehensions/snapshots/ruff_linter__rules__flake8_comprehensions__tests__C416_C416.py.snap`:111 on 2024-12-17 13:52_

`d` might look like `{"list": [1, 2]}`. If so, it can't be fixed to `dict(d.items())`.

---

_@harupy reviewed on 2024-12-17 13:52_

---

_Comment by @dhruvmanila on 2024-12-19 12:36_

Looking at this now

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:151 on 2024-12-19 12:47_

```suggestion
            // [(k, v) for k, v in dictionary.items()]
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:156 on 2024-12-19 12:48_

Do we need to check for list as well?

---

_@dhruvmanila approved on 2024-12-19 12:50_

Looks good. It's a bummer that we have to move away from `ComparableExpr` which adds a lot of complexity around matching and extracting the values.

---

_@harupy reviewed on 2024-12-19 14:14_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:151 on 2024-12-19 14:14_

Thanks for the catch!

---

_@harupy reviewed on 2024-12-19 14:15_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension.rs`:156 on 2024-12-19 14:15_

Yes, `target` can be a list. Thanks for the catch :)

---

_Review requested from @dhruvmanila by @harupy on 2024-12-19 14:15_

---

_Comment by @dylwil3 on 2024-12-19 20:41_

Does this also solve #4491 ? Could you add a test demonstrating that if so?

---

_Comment by @harupy on 2024-12-20 07:23_

@dylwil3 No, it doesn't solve that issue. To solve that issue, I think type inference is required.

---

_Merged by @dhruvmanila on 2024-12-20 09:05_

---

_Closed by @dhruvmanila on 2024-12-20 09:05_

---

_Branch deleted on 2024-12-20 09:50_

---
