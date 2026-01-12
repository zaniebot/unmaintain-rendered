```yaml
number: 7811
title: "[pylint] Implement consider-using-ternary (R1706)"
type: pull_request
state: merged
author: danbi2990
labels:
  - rule
assignees: []
merged: true
base: main
head: consider-using-ternary
created_at: 2023-10-04T12:57:49Z
updated_at: 2023-10-13T05:44:42Z
url: https://github.com/astral-sh/ruff/pull/7811
synced_at: 2026-01-12T02:32:41Z
```

# [pylint] Implement consider-using-ternary (R1706)

---

_Pull request opened by @danbi2990 on 2023-10-04 12:57_

This is my first PR. Please feel free to give me any feedback for even small drawbacks.

## Summary

Checks if pre-python 2.5 ternary syntax is used.

Before
```python
x, y = 1, 2
maximum = x >= y and x or y  # [consider-using-ternary]
```

After
```python
x, y = 1, 2
maximum = x if x >= y else y
```

References: 
[pylint](https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/consider-using-ternary.html)
#970 
[and_or_ternary distinction logic](https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/refactoring/refactoring_checker.py#L1813)

## Test Plan

Unit test, python file, snapshot added.

---

_Comment by @codspeed-hq[bot] on 2023-10-04 13:26_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/danbi2990:consider-using-ternary)

### Merging #7811 will **not alter performance**

<sub>Comparing <code>danbi2990:consider-using-ternary</code> (2551cca) with <code>main</code> (6f9c317)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-10-04 14:05_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -1, 0 error(s))

<details><summary>rotki (+2, -1)</summary>
<p>

<pre>
- [*] 11 fixable with the `--fix` option (20 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 11 fixable with the `--fix` option (21 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/rotki/rotki/blob/462f4965c8ab2a73a42a0312816eabf5e31b604c/rotkehlchen/tests/fixtures/accounting.py#L56'>rotkehlchen/tests/fixtures/accounting.py:56:13:</a> PLR1706 Consider using if-else expression
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR1706 | 1 | 1 | 0 |



---

_Converted to draft by @danbi2990 on 2023-10-05 00:49_

---

_Marked ready for review by @danbi2990 on 2023-10-05 07:17_

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/helpers.rs`:94 on 2023-10-06 12:46_

You can avoid the `.unwrap()` using let-else:

```suggestion
    if let Some(and_op) = and_op.and_op.as_bool_op_expr() else {
        return false;
    }
    let and_values = and_op.values;
    let false_value = &bool_op.values[1];
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/helpers.rs`:97 on 2023-10-06 12:49_

Similarly, you can avoid this check with let-else:

```rust
let [and_op, false_value] = bool_op.values.as_slice() == 2 else {
    return false;
}
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/helpers.rs`:104 on 2023-10-06 12:50_

```suggestion
#[cfg(test)]
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/helpers.rs`:113 on 2023-10-06 12:50_

```suggestion
#[cfg(test)]
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/helpers.rs`:123 on 2023-10-06 12:53_

From a general rust perspective, having unit tests in a file is good practice, but for the ruff linter specifically we generally only use the python test file because it makes more sense for us (mostly because we would test the same thing in the unit tests that we also test in the python files, so we'd only get code we have to keep in sync without additional test coverage)

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:14 on 2023-10-06 12:54_

```suggestion
/// If-expressions are more readable than logical ternary expressions.
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:19 on 2023-10-06 12:54_

```suggestion
/// maximum = x >= y and x or y
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:38 on 2023-10-06 12:58_

```suggestion
        format!("Pre-python 2.5 ternary syntax used")
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:56 on 2023-10-09 10:11_

you could return an option from `is_and_or_ternary` which contains the test, body and orelse so you can avoid the unwrapping.

---

_@konstin reviewed on 2023-10-09 10:11_

---

_Comment by @danbi2990 on 2023-10-10 10:47_

@konstin 
All comments applied. Thank you!

---

_Review requested from @konstin by @danbi2990 on 2023-10-10 10:47_

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/helpers.rs`:88 on 2023-10-10 15:00_

Here i'd return `Option<(Expr, Expr, Expr)>`. It's the same from the perspective of the compiler, but it makes it clearer that these are different parts of the code that only incidentally have the same type.

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/helpers.rs`:105 on 2023-10-10 15:07_

Personal style preference:

```suggestion
    let Some(and_op) = expr.as_bool_op_expr() else {
        return None;
    }
    if and_op.op != BoolOp::And {
        return None;
    }   
    let [condition, true_value] = and_op.values.as_slice() else {
        return None;
    };
    if !false_value.is_bool_op_expr() && !true_value.is_bool_op_expr() {
        return Some([condition.clone(), true_value.clone(), false_value.clone()]);
    }
}
```

With a different reviewer you would likely get a slightly different structure, but we normally prefer some kind of flatter structure.

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/helpers.rs`:86 on 2023-10-10 15:09_

I'd move this function to `and_or_ternary.rs` (and make it private) since it's not used anywhere else.

---

_@konstin reviewed on 2023-10-10 15:24_

Thanks for the fixes, i have added some more notes for the next iteration.

I've also looked through the ecosystem checks; I think we shouldn't warn when the parent is an if statement (e.g. https://github.com/apache/airflow/blob/609eed90698aa7abeb5bae9ae156062d32baae86/airflow/models/dag.py#L2639 or https://github.com/rotki/rotki/blob/7e88ac61f98e21e231b43186a6fbee2e090b1be5/package.py#L1070) or when the parent is a binary expression (e.g. https://github.com/bokeh/bokeh/blob/cf57b9ae0073ecf0434a8c10002a70f6bf2079cd/src/bokeh/core/property/dataspec.py#L588).

[This line](https://github.com/rotki/rotki/blob/7e88ac61f98e21e231b43186a6fbee2e090b1be5/rotkehlchen/tests/fixtures/accounting.py#L56) is also not a case of R1706, but the original code looks wrong, i'm pretty sure they meant
```python
x.is_dir() and ((x / 'rotkehlchen.db').exists() or (x / 'rotkehlchen_transient.db').exists())
```
instead of
```python
x.is_dir() and (x / 'rotkehlchen.db').exists() or (x / 'rotkehlchen_transient.db').exists()
```
, so imo that's fine.

---

_Comment by @danbi2990 on 2023-10-11 05:06_

@konstin 
All comments applied. Thank you. 🙇‍♂️

---

_Review requested from @konstin by @danbi2990 on 2023-10-11 05:06_

---

_Comment by @konstin on 2023-10-11 12:55_

I found a case where the fix doesn't work (https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/airflow/utils/setup_teardown.py#L199):
```python
new_list = [
    x
    for sublist in all_lists
    for x in sublist
    if (isinstance(operator, list) and x in operator) or x != operator
]
```
is fixed to 
```python
new_list = [
    x
    for sublist in all_lists
    for x in sublist
    if x in operator if isinstance(operator, list) else x != operator
]
```
which is invalid syntax, the expression needs parentheses here. @charliermarsh Do we have a util to detect whether we need to add parentheses when converting a bool op to a ternary expression?

---

_Comment by @danbi2990 on 2023-10-12 15:32_

@konstin 
How about this version? 🤔

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:118 on 2023-10-12 16:36_

```suggestion
        diagnostic.set_fix(Fix::unsafe_edit(Edit::range_replacement(
```

I'm not certain that we have captured all cases where this could leads to invalid syntax but i also couldn't enumerate which cases they are and also the fix is new, so i'd go with an unsafe fix.

---

_@konstin approved on 2023-10-12 16:36_

Looks good!

---

_Review requested from @charliermarsh by @konstin on 2023-10-12 16:37_

---

_@danbi2990 reviewed on 2023-10-13 00:18_

---

_Review comment by @danbi2990 on `crates/ruff_linter/src/rules/pylint/rules/and_or_ternary.rs`:118 on 2023-10-13 00:18_

That makes sense. Applied. 👍

---

_Label `rule` added by @charliermarsh on 2023-10-13 01:18_

---

_@charliermarsh approved on 2023-10-13 01:19_

Thank you!

---

_Merged by @charliermarsh on 2023-10-13 01:29_

---

_Closed by @charliermarsh on 2023-10-13 01:29_

---

_Branch deleted on 2023-10-13 05:44_

---
