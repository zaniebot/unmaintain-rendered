```yaml
number: 15955
title: "[`flake8-comprehensions`] Detect overshadowed `list`/`set`/`dict`, ignore variadics and named expressions (`C417`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: C417-2
created_at: 2025-02-05T01:01:21Z
updated_at: 2025-02-07T15:12:15Z
url: https://github.com/astral-sh/ruff/pull/15955
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-comprehensions`] Detect overshadowed `list`/`set`/`dict`, ignore variadics and named expressions (`C417`)

---

_@InSyncWithFoo_

## Summary

Part of #15809 and #15876.

This change brings several bugfixes:

* The nested `map()` call in `list(map(lambda x: x, []))` where `list` is overshadowed is now correctly reported.
* The call will no longer reported if:
	* Any arguments given to `map()` are variadic.
	* Any of the iterables contain a named expression.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-05 01:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2025-02-05 08:52_

---

_Label `rule` added by @MichaReiser on 2025-02-05 08:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:652 on 2025-02-05 09:37_

I'd prefer to keep passing `expr` here (or even `CallExpr`) because it is more meaningful than just `range`. `range` itself doesn't convey any information to reader. What is it the range of? Or is

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:102 on 2025-02-05 09:42_

This check is now redundant with the check on line 71 or was it unintentional to move the check to line 71 because it wasn't applied for the `Generator` case before

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:138 on 2025-02-05 09:45_

Nit: We could do this in a single iteration
```suggestion
    // For example, (x+1 for x in (c:=a)) is invalid syntax
    // so we can't suggest it.
    for iterable in &iterables {
        if iterable.is_starred_expr() {
            return;
        }

        if any_over_expr(iterable, &|expr| expr.is_named_expr()) {
            return
        }
    }
```

or 

```suggestion
    // For example, (x+1 for x in (c:=a)) is invalid syntax
    // so we can't suggest it.
    if iterables
        .iter()
        .any(|iterable| iterable.is_starred_expr() || any_over_expr(iterable, &|expr| expr.is_named_expr()))
    {
        return;
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:184 on 2025-02-05 09:49_

Nit
```suggestion
    let Some((Expr::Lambda(lambda), iterables)) = arguments.args.split_first() else {
        return None;
    };
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:175 on 2025-02-05 09:50_

I suggest to remove the lambda here, it reduces the tuple arity and makes it clearer what the second expression is
```suggestion
fn map_lambda_and_iterables<'a>(
    call: &'a ast::ExprCall,
    semantic: &'a SemanticModel,
) -> Option<(&'a ExprLambda, &'a [Expr])> {
```

It also allows to simplify `lambda_has_expected_arity` to `lambda_has_expected_arity(lambda)` and you can move the check if `parameters` is `Some` into `lambda_has_expected_arity`

---

_@MichaReiser reviewed on 2025-02-05 09:56_

Thank you. It's sort of difficult to see the actual fixes in between the refactors you did. Would you mind commenting on the diff to highlight the fixes for each bullet point?

---

_@InSyncWithFoo reviewed on 2025-02-06 19:06_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:175 on 2025-02-06 19:06_

I think I intentionally left it this way so that #15876's `fix_unnecessary_map_preview()` could make use of `parameters` directly (i.e., avoid destructuring `parameters` twice).

---

_Comment by @InSyncWithFoo on 2025-02-07 00:49_

> Would you mind commenting on the diff to highlight the fixes for each bullet point?

The diff is a tad too big to reason about, so here's something else to the same effect.

---

> The nested `map()` call in `list(map(lambda x: x, []))` where `list` is overshadowed is now correctly reported.

[Before](https://github.com/astral-sh/ruff/blob/10d3e64ccdf69d90c3252a15b3408a4238427792/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs#L92-L98):

```rust
if parent
    .and_then(Expr::as_call_expr)
    .and_then(|call| call.func.as_name_expr())
    .is_some_and(|name| matches!(name.id.as_str(), "list" | "set" | "dict"))
{
    return;
}
```

[After](https://github.com/InSyncWithFoo/ruff/blob/e14b8664043339a34c8450ceb136a3dd1123d5c1/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs#L89-L91):

```rust
if parent_call_func.is_some_and(|func| is_list_set_or_dict(func, semantic)) {
    return;
}

// ...

fn is_list_set_or_dict(func: &Expr, semantic: &SemanticModel) -> bool {
    semantic
        .resolve_qualified_name(func)
        .is_some_and(|qualified_name| {
            matches!(
                qualified_name.segments(),
                ["" | "builtins", "list" | "set" | "dict"]
            )
        })
}
```

> * The call will no longer reported if:
> 	* Any arguments given to `map()` are variadic.
> 	* Any of the iterables contain a named expression.

This check [was present](https://github.com/astral-sh/ruff/blob/10d3e64ccdf69d90c3252a15b3408a4238427792/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs#L108-L112), but only for the `::Generator` branch:

```rust
// For example, (x+1 for x in (c:=a)) is invalid syntax
// so we can't suggest it.
if any_over_expr(iterable, &|expr| expr.is_named_expr()) {
    return;
}
```

Now, it is placed [outside of the branches](https://github.com/InSyncWithFoo/ruff/blob/e14b8664043339a34c8450ceb136a3dd1123d5c1/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs#L127-L137), along with a new check for starred expressions:

```rust
for iterable in iterables {
    // For example, (x+1 for x in (c:=a)) is invalid syntax
    // so we can't suggest it.
    if any_over_expr(iterable, &|expr| expr.is_named_expr()) {
        return;
    }

    if iterable.is_starred_expr() {
        return;
    }
}
```

Similarly, other pieces that are applicable to all branches have also been moved into [`map_lambda_and_iterables()`](https://github.com/InSyncWithFoo/ruff/blob/e14b8664043339a34c8450ceb136a3dd1123d5c1/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs#L168-L187).

---

_@MichaReiser approved on 2025-02-07 08:55_

Perfect and thanks for the explanations. They were very helpful when reviewing the PR

---

_Merged by @MichaReiser on 2025-02-07 08:58_

---

_Closed by @MichaReiser on 2025-02-07 08:58_

---

_Branch deleted on 2025-02-07 15:12_

---
