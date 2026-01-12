```yaml
number: 13435
title: "[pylint] Implement `boolean-chained-comparison` (`R1716`)"
type: pull_request
state: merged
author: vincevannoort
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pylint/chained-comparison
created_at: 2024-09-21T10:25:28Z
updated_at: 2024-09-25T09:23:51Z
url: https://github.com/astral-sh/ruff/pull/13435
synced_at: 2026-01-12T15:55:44Z
```

# [pylint] Implement `boolean-chained-comparison` (`R1716`)

---

_@vincevannoort_

## Summary

This pull request implements the `R1716` pylint rule: [pylint documentation](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/chained-comparison.html).

It checks boolean `and` operations that can can be chained

```python
a = int(input())
b = int(input())
c = int(input())
if a < b and b < c:
    pass
```

And suggest to combine them:

```python
a = int(input())
b = int(input())
c = int(input())
if a < b < c:
    pass
```

## Test Plan

I have added test cases, and checked the `ruff ecosystem` results.


---

_Renamed from "[pylint] Implement `boolean-chained_comparison` (`R1716`)" to "[pylint] Implement `boolean-chained-comparison` (`R1716`)" by @vincevannoort on 2024-09-21 10:28_

---

_Comment by @github-actions[bot] on 2024-09-21 11:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b9b7bfc6a8dff0324b00b8fb2a39624ad213a4c2/airflow/utils/usage_data_collection.py#L120'>airflow/utils/usage_data_collection.py:120:12:</a> PLR1716 [*] Contains chained boolean comparison that can be simplified
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1716 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @vincevannoort on 2024-09-21 11:07_

I notice there are some false positives that I will need to address in the ecosystem checks

---

_Marked ready for review by @vincevannoort on 2024-09-22 07:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:260 on 2024-09-23 09:12_

```suggestion
        (Pylint, "R1716") => (RuleGroup::Preview, rules::pylint::rules::BooleanChainedComparison),
```

New rules should be in the preview group so that we can get feedback first.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:81 on 2024-09-23 09:15_

Nit: The `collect` of compare expressions is unnecessary
```suggestion
 let compare_exprs = expr_bool_op
        .values
        .iter()
        .map(|stmt| stmt.as_compare_expr().unwrap());

    let results: Vec<Result<(), BooleanChainedComparison>> = compare_exprs
        .tuple_windows()
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:106 on 2024-09-23 09:25_

Instead of using an `Err` here, I suggest using `filter_map` where you return `None` when there's no `Diagnostic and `Some` otherwise. That also reduces the need for the `results` on line 109-113
```suggestion
        .filter_map(|(left_compare, right_compare)| {
            let Expr::Name(left_compare_right) = left_compare.comparators.first()? else {
                return None;
            };

            let Expr::Name(right_compare_left) = &*right_compare.left else {
                return None;
            };

            if left_compare_right.id() != right_compare_left.id() {
                return None;
            }

            Some(BooleanChainedComparison {
                variable: left_compare_right.id().clone(),
                range: TextRange::new(left_compare.range.start(), right_compare.range.end()),
                replace_range: TextRange::new(
                    left_compare_right.range.start(),
                    right_compare_left.range.end(),
                ),
            })
        });

```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:119 on 2024-09-23 09:26_

The clone here should be unnecessary, considering that you call `to_string` on line 124

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:126 on 2024-09-23 09:27_

Nit: I'm inclined to inline those variables as they're only used once (simplifies reading because there's less aliasing that my mind has to keep track of)

```suggestion
                let mut diagnostic = Diagnostic::new(result, result.range);
                diagnostic.set_fix(Fix::safe_edit(Edit::range_replacement(
                    variable.to_string(),
                    result.replace_range,
                )));
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:129 on 2024-09-23 09:27_

The `collect` here should be unnecessary. `extend` takes any iterator argument

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:70 on 2024-09-23 09:31_

I think this can happen

```python
if a < b and not c and b < c:
	pass

```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:60 on 2024-09-23 09:33_

I don't think this is necessary. A bool expression with less than 2 values isn't a valid bool expression (`a and` isn't valid syntax)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:50 on 2024-09-23 09:34_

Simplify is very unspecific and it's not clear to me as a reader what I should change. 

Maybe: Use a single compare expression



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:144 on 2024-09-23 09:36_

```suggestion
		let left_operator = left.ops.as_slice() else {
			return false;
		}
		
		let right_operator = right.ops.as_slice() else {
			return false;
		}

        matches!(
        (left_operator, right_operator),
        (CmpOp::Lt | CmpOp::LtE, CmpOp::Lt | CmpOp::LtE)
            | (CmpOp::Gt | CmpOp::GtE, CmpOp::Gt | CmpOp::GtE)
    )
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:104 on 2024-09-23 09:41_

You can import the `Ranged` trait and then call` start` and `end` directly on the nodes

```suggestion
                    left_compare_right.start(),
                    right_compare_left.end(),
                ),
```

---

_@MichaReiser requested changes on 2024-09-23 09:45_

This is great. I've a few nits that should help improve the performance of this rule. 

---

_Label `rule` added by @MichaReiser on 2024-09-23 09:45_

---

_Label `preview` added by @MichaReiser on 2024-09-23 09:45_

---

_@vincevannoort reviewed on 2024-09-24 05:21_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:126 on 2024-09-24 05:21_

I was able to move the creation of the edit up like this:

```rust
            let range = result.range;
            let edit = Edit::range_replacement(result.variable.to_string(), result.replace_range);
            let mut diagnostic = Diagnostic::new(result, range);
                        diagnostic.set_fix(Fix::safe_edit(edit));
```

Preventing the `let variable = result.variable.clone();` and `let replace_range = result.replace_range;`.

However, when trying to do the suggested `Diagnostic::new(result, result.range);` I get borrow checker errors:

```rust
            let edit = Edit::range_replacement(result.variable.to_string(), result.replace_range);
            let mut diagnostic = Diagnostic::new(result, result.range);
            diagnostic.set_fix(Fix::safe_edit(edit));
```

Gives me these errors:

```rust
error[E0382]: use of moved value: `result`
   --> crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs:104:58
    |
102 |         .extend(results.into_iter().map(|result| {
    |                                          ------ move occurs because `result` has type `rules::pylint::rules::boolean_chained_comparison::BooleanChainedComparison`, which does not implement the `Copy` trait
103 |             let edit = Edit::range_replacement(result.variable.to_string(), result.replace_range);
104 |             let mut diagnostic = Diagnostic::new(result, result.range);
    |                                                  ------  ^^^^^^^^^^^^ value used here after move
    |                                                  |
    |                                                  value moved here
```

What do you think would be the best thing to do here?

---

_@vincevannoort reviewed on 2024-09-24 05:29_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:144 on 2024-09-24 05:29_

First this mentions this:

```
error[E0599]: no method named `as_slice` found for struct `std::boxed::Box<[ruff_python_ast::CmpOp]>` in the current scope
```

Then I import the trait: `use core::slice::SlicePattern;` for it to work, I get the following error:

```
error[E0658]: use of unstable library feature 'slice_pattern': stopgap trait for slice patterns
```

---

_Comment by @vincevannoort on 2024-09-24 05:32_

> This is great. I've a few nits that should help improve the performance of this rule.

@MichaReiser thanks so much for thorough feedback! 👌 

I have fixed most of the review comments, but two are still open where I need some extra help from you.

---

_Review requested from @MichaReiser by @vincevannoort on 2024-09-24 05:32_

---

_@vincevannoort reviewed on 2024-09-24 05:49_

---

_Review comment by @vincevannoort on `crates/ruff_linter/resources/test/fixtures/pylint/boolean_chained_comparison.py`:120 on 2024-09-24 05:49_

This to me does seem like a case where it could be written as a single expression `a > b < c`, but I am not entirely sure. 

---

_@MichaReiser reviewed on 2024-09-24 12:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:144 on 2024-09-24 12:00_

Oh sorry, I incorrectly assumed that `left.ops` is a `Vec` but it's a `Box<[]>`. The following should work (dereference the Box to `[..]` and then take a reference of it):

```rust
let left_operator = &*left.ops else {
      return false;
  };
```

---

_@MichaReiser reviewed on 2024-09-24 12:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:126 on 2024-09-24 12:02_

Oh, I didn't notice that `result` is used in `Diagnostic::new`. Assigning a temporary variable is then indeed necessary to satisfy the borrow checker.

---

_@vincevannoort reviewed on 2024-09-25 05:56_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:144 on 2024-09-25 05:56_

I like it, implemented here, but adjusted slightly to match the single operation in the array:

```rs
let [left_operator] = &*left.ops else {
    return false;
};
```

---

_@MichaReiser reviewed on 2024-09-25 08:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/boolean_chained_comparison.rs`:144 on 2024-09-25 08:50_

oh my sorry. Yeah my code is just wrong. It needs the `[left_operator]`. 

---

_@MichaReiser reviewed on 2024-09-25 09:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/boolean_chained_comparison.py`:120 on 2024-09-25 09:08_

Possibly, but I'm not convinced that it improves readability. 

---

_@MichaReiser approved on 2024-09-25 09:09_

Thanks, this is great

---

_Merged by @MichaReiser on 2024-09-25 09:14_

---

_Closed by @MichaReiser on 2024-09-25 09:14_

---
