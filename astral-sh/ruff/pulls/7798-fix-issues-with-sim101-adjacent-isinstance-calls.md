```yaml
number: 7798
title: Fix issues with SIM101 (adjacent isinstance() calls)
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - bug
assignees: []
merged: true
base: main
head: sim101
created_at: 2023-10-04T03:50:38Z
updated_at: 2024-09-10T23:38:39Z
url: https://github.com/astral-sh/ruff/pull/7798
synced_at: 2026-01-12T15:55:24Z
```

# Fix issues with SIM101 (adjacent isinstance() calls)

---

_@JelleZijlstra_

- Only trigger for immediately adjacent isinstance() calls with the same target
- Preserve order of or conditions

Two existing tests changed:
- One was incorrectly reordering the or conditions, and is now correct.
- Another was combining two non-adjacent isinstance() calls. It's safe enough in that example,
  but this isn't safe to do in general, and it feels low-value to come up with a heuristic for
  when it is safe, so it seems better to not combine the calls in that case.

Fixes https://github.com/astral-sh/ruff/issues/7797

---

_@charliermarsh reviewed on 2023-10-04 04:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:365 on 2023-10-04 04:11_

Perhaps instead:

```rust
if last_target_option
    .as_ref()
    .is_some_and(|last_target| *last_target == ComparableExpr::from(target))
{
    duplicates
        .last_mut()
        .expect("last_target should have a corresponding entry")
        .push(index);
} else {
    last_target_option = Some(target.into());
    duplicates.push(vec![index]);
}
```

A few things, all minor:

1. Chaining the option via `is_some_and` rather than nesting the `if` statements.
2. Using `ComparableExpr::from` instead of `Into` with the filled-in generic.
3. (1) allows us to use an `if`-`else` rather than a `continue`, which is nice.

---

_@charliermarsh reviewed on 2023-10-04 04:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:449 on 2023-10-04 04:11_

(I know it was already written this way, but I think `.map(Clone::clone)` can be replaced with `.cloned()`.)

---

_@charliermarsh reviewed on 2023-10-04 04:14_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:448 on 2023-10-04 04:14_

An alternative that would avoid some index math:

```rust
let [first, .., last] = indices.as_slice() else {
    unreachable!("Indices should have at least two elements")
};
let before = values.iter().take(*first).cloned();
let after = values.iter().skip(last + 1).cloned();
```

But don't feel strongly.

---

_@JelleZijlstra reviewed on 2023-10-04 04:15_

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:365 on 2023-10-04 04:15_

Thanks, you can probably tell that this is the first serious Rust code I've ever written :) I went over the Rust book a few years ago but haven't had a chance to write any code.

I applied both of your suggestions and they work, will push in a moment.

---

_@JelleZijlstra reviewed on 2023-10-04 04:15_

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:448 on 2023-10-04 04:15_

Thanks, that does feel cleaner.

---

_@charliermarsh reviewed on 2023-10-04 04:18_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:349 on 2023-10-04 04:18_

I'd love to avoid these but it's a bit tedious to do so...

One option: add a method like:

```rust
fn isinstance_target(call: &Expr, semantic: &SemanticModel) -> Option<&Expr> {
    // Verify that this is an `isinstance` call.
    let Expr::Call(ast::ExprCall {
        func,
        arguments:
            Arguments {
                args,
                keywords,
                range: _,
            },
        range: _,
    }) = &call
    else {
        return None;
    };
    if args.len() != 2 {
        return None;
    }
    if !keywords.is_empty() {
        return None;
    }
    let Expr::Name(ast::ExprName { id: func_name, .. }) = func.as_ref() else {
        return None;
    };
    if func_name != "isinstance" {
        return None;
    }
    if !semantic.is_builtin("isinstance") {
        return None;
    }

    // Collect the target (e.g., `obj` in `isinstance(obj, int)`).
    Some(&args[0])
}
```

Then this becomes:
```rust
let Some(target) = isinstance_target(&call, checker.semantic()) else {
    last_target_option = None;
    continue;
};
```

---

_@charliermarsh approved on 2023-10-04 04:18_

---

_@charliermarsh reviewed on 2023-10-04 04:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:365 on 2023-10-04 04:19_

Haha, if this is the first serious Rust code you've ever written, it's very impressive that you can make sense of this file!

---

_@JelleZijlstra reviewed on 2023-10-04 04:22_

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:349 on 2023-10-04 04:22_

Yes it's pretty repetitive, I will apply your change.

---

_@charliermarsh reviewed on 2023-10-04 04:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_bool_op.rs`:349 on 2023-10-04 04:24_

Sounds good, will wait for that then merge. Thanks!

---

_Label `bug` added by @charliermarsh on 2023-10-04 04:26_

---

_Comment by @charliermarsh on 2023-10-04 04:26_

Looks great, thanks @JelleZijlstra!

---

_Merged by @charliermarsh on 2023-10-04 04:42_

---

_Closed by @charliermarsh on 2023-10-04 04:42_

---

_Comment by @codspeed-hq[bot] on 2023-10-04 04:46_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/JelleZijlstra:sim101)

### Merging #7798 will **improve performances by 2.42%**

<sub>Comparing <code>JelleZijlstra:sim101</code> (3cea5ec) with <code>main</code> (5d49d26)</sub>



### Summary

`⚡ 2` improvements
`✅ 23` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `JelleZijlstra:sim101` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 38.9 ms | 38 ms | +2.42% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 35.4 ms | 34.7 ms | +2.17% |


---

_Comment by @github-actions[bot] on 2023-10-04 04:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Branch deleted on 2023-10-04 04:49_

---

_Branch restored on 2024-09-10 23:38_

---
