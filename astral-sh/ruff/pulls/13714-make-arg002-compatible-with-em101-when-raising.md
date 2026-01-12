```yaml
number: 13714
title: "Make `ARG002` compatible with `EM101` when raising `NotImplementedError`"
type: pull_request
state: merged
author: Watercycle
labels:
  - bug
assignees: []
merged: true
base: main
head: arg002-em101-compatibility
created_at: 2024-10-11T07:22:16Z
updated_at: 2024-10-20T06:27:14Z
url: https://github.com/astral-sh/ruff/pull/13714
synced_at: 2026-01-12T15:55:45Z
```

# Make `ARG002` compatible with `EM101` when raising `NotImplementedError`

---

_@Watercycle_

## Summary

This pull request resolves some rule thrashing identified in #12427 by allowing for unused arguments when using `NotImplementedError` with a variable per [this comment](https://github.com/astral-sh/ruff/issues/12427#issuecomment-2384727468).

**Note**

This feels a little heavy-handed / edge-case-prone. So, to be clear, I'm happy to scrap this code and just update the docs to communicate that `abstractmethod` and friends should be used in this scenario (or similar). Just let me know what you'd like done!

fixes: #12427 

## Test Plan

I added a test-case to the existing `ARG.py` file and ran...

```sh
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_unused_arguments/ARG.py --no-cache --preview --select ARG002
```

---

_Comment by @github-actions[bot] on 2024-10-11 07:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:331 on 2024-10-14 09:17_

Nit:

```suggestion
            if matches!(**value, ast::Expr::StringLiteral(_)) =>
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:347 on 2024-10-14 09:17_

Nit:
```suggestion
        **value,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:359 on 2024-10-14 09:18_

Nit:
```suggestion
    let [Stmt::Assign(ast::StmtAssign { value, .. }), Stmt::Raise(StmtRaise {
        exc: Some(exception),
        ..
    })] = statements
    else {
        return false;
    };
    
    if !matches!(
        value.as_ref(),
        ast::Expr::StringLiteral(_) | ast::Expr::FString(_)
    ) {
        return false;
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:363 on 2024-10-14 09:19_

Nit:
```suggestion
    }) = &**exception
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:368 on 2024-10-14 09:20_

```suggestion
    if !matches!(**func, ast::Expr::Name(name) if name.id == "NotImplementedError") {
```

---

_@MichaReiser approved on 2024-10-14 09:20_

This looks good to me. Would you mind adding a test string with an f-string message?

---

_Label `rule` added by @MichaReiser on 2024-10-14 09:20_

---

_@Watercycle reviewed on 2024-10-16 02:18_

---

_Review comment by @Watercycle on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:363 on 2024-10-16 02:18_

I see mixed usages of `&**` and `as_ref` throughout the code base. Out of curiosity, when is one used over the other? The general heuristic I've seen is `*` and `&` are recommended for simple stuff; and, once you start de-referencing and re-borrowing, that `as_ref` is recommended if just to avoid needing to think about whatever container the value* is in.

---

_Review requested from @MichaReiser by @Watercycle on 2024-10-16 02:19_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:334 on 2024-10-16 04:16_

nit: we could directly use the `is_string_literal_expr` method here
```suggestion
        [Stmt::Expr(StmtExpr { value, .. }), rest @ ..] if value.is_string_literal_expr() => rest,
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:366 on 2024-10-16 04:26_

I think we'd also need to make sure that the variable that is being referenced in the call to `NotImplementedError` is the same as the one in the assignment. I think there must be some method in the `SemanticModel` to get the binding for the `ExprName` node (possibly `resolve_name`?).

Or, actually we can get away with just comparing the string i.e., whether the name of the variable that is being assigned is the same as the one used in the `NotImplementedError` call because we make sure that there are only two statements in the function body above.

---

_@dhruvmanila reviewed on 2024-10-16 04:28_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-10-18 05:57_

---

_@dhruvmanila approved on 2024-10-18 06:03_

Thanks for the PR!

I made a few final changes:
* We should match the assignment target and argument node exactly. Previously, if either of them isn't a name node, the function would return true.
* Added some more test cases where `ARG002` should be raised

---

_Comment by @dhruvmanila on 2024-10-18 06:05_

Going to wait for ecosystem checks and then merge.

---

_Renamed from "Make ARG002 compatible with EM101 (no unused args vs no raw exception message)" to "Make `ARG002` compatible with `EM101` when raising `NotImplementedError`" by @dhruvmanila on 2024-10-18 06:29_

---

_@MichaReiser reviewed on 2024-10-18 06:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:363 on 2024-10-18 06:31_

We used to use `as_ref` but started to prefer dereferencing (except for `Option` where you still need `as_ref` and `as_deref`) because it's possible that `as_ref` becomes ambiguous if a new `AsRef` implementation is added to a type. For example, `String` has many `as_ref` methods:

```
  = note: multiple `impl`s satisfying `String: AsRef<_>` found in the following crates: `alloc`, `std`:
          - impl AsRef<OsStr> for String;
          - impl AsRef<Path> for String;
          - impl AsRef<[u8]> for String;
          - impl AsRef<str> for String;
```

---

_Label `rule` removed by @dhruvmanila on 2024-10-18 06:31_

---

_Label `bug` added by @dhruvmanila on 2024-10-18 06:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:355 on 2024-10-18 06:32_

We should probably use the semantic model to verify that `NotImplementedError` is the built-in `NotImplementedError`

---

_@MichaReiser reviewed on 2024-10-18 06:32_

---

_@dhruvmanila reviewed on 2024-10-18 06:32_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:355 on 2024-10-18 06:32_

Oh, good catch.

---

_Merged by @dhruvmanila on 2024-10-18 06:44_

---

_Closed by @dhruvmanila on 2024-10-18 06:44_

---

_Branch deleted on 2024-10-20 06:03_

---

_@Watercycle reviewed on 2024-10-20 06:19_

---

_Review comment by @Watercycle on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:363 on 2024-10-20 06:19_

Oh interesting, that makes sense! Thank you!

---

_Comment by @Watercycle on 2024-10-20 06:27_

> I made a few final changes:

Thank you for fixing it up! I noticed the snapshot test didn't get updated after committing and thought "this can be Sunday's problem" 😅 

---
