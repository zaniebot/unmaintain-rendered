```yaml
number: 7999
title: "[pylint] - add `unnecessary-list-index-lookup` (`PLR1736`) + autofix"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-R1736
created_at: 2023-10-17T01:50:32Z
updated_at: 2023-12-01T00:34:04Z
url: https://github.com/astral-sh/ruff/pull/7999
synced_at: 2026-01-12T15:55:25Z
```

# [pylint] - add `unnecessary-list-index-lookup` (`PLR1736`) + autofix

---

_@diceroll123_

## Summary

Add [R1736](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/unnecessary-list-index-lookup.html) along with the autofix

See #970 

## Test Plan

`cargo test` and manually

---

_Comment by @github-actions[bot] on 2023-10-17 02:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Renamed from "[pylint] add `unnecessary-list-index-lookup` `PLR1736` with autofix" to "[pylint] - add `unnecessary-list-index-lookup` (`PLR1736`) + autofix" by @diceroll123 on 2023-10-18 04:12_

---

_Comment by @Skylion007 on 2023-11-04 17:34_

@diceroll123 I tried out your rule and found a bug:

Consider this code snippit
```python
a = list(range(5, 10))
for i, j in enumerate(a):
    a[i] = j + 1
    assert a[i] > j
```
becomes transformed to:
```python
a = list(range(5, 10))
for i, j in enumerate(a):
    a[i] = j + 1
    assert j > j
```
which now outputs an Assertion Error. In other words, you have to make sure `a[i]` is always an alias of `j` which is not the case here. I suspect your other PR has a similar issue.

You should also test mutating j and changing the alias there.

ie:
```python
j = j+1
assert a[i] != j
```

---

_Comment by @diceroll123 on 2023-11-04 18:15_

@Skylion007 good catch, thanks! Will update.

I think the safest and sanest way forward is to stop emitting once the visitor finds a subscript assignment to the currently-looped value.

---

_Comment by @github-actions[bot] on 2023-11-04 18:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/eed6427b66e5af51d7e6ff4afb5f7115b2754cf3/dev/breeze/src/airflow_breeze/utils/parallel.py#L297'>dev/breeze/src/airflow_breeze/utils/parallel.py:297:57:</a> PLR1736 [*] Unnecessary lookup of list item by index
+ <a href='https://github.com/apache/airflow/blob/eed6427b66e5af51d7e6ff4afb5f7115b2754cf3/dev/breeze/src/airflow_breeze/utils/parallel.py#L299'>dev/breeze/src/airflow_breeze/utils/parallel.py:299:40:</a> PLR1736 [*] Unnecessary lookup of list item by index
+ <a href='https://github.com/apache/airflow/blob/eed6427b66e5af51d7e6ff4afb5f7115b2754cf3/scripts/ci/pre_commit/pre_commit_check_provider_airflow_compatibility.py#L51'>scripts/ci/pre_commit/pre_commit_check_provider_airflow_compatibility.py:51:20:</a> PLR1736 [*] Unnecessary lookup of list item by index
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1736 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:222 on 2023-11-27 16:22_

Should this use `checker.semantic().resolve_call_path(func)`?

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:15 on 2023-11-27 16:25_

Perhaps
```suggestion
/// It is more succinct to use the variable for the value at the current index which is already in scope from the iterator. 
```

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:12 on 2023-11-27 16:26_

Perhaps
```suggestion
/// Checks for access of a list item at the current index when using enumeration.
```

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:38 on 2023-11-27 16:27_

```suggestion
        format!("Unnecessary lookup of list item by index")
```

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:42 on 2023-11-27 16:27_

```suggestion
        format!("Use existing item variable instead")
```

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_list_index_lookup.py`:19 on 2023-11-27 16:28_

What about a case where `index` is mutated? e.g. `index += 1`

---

_@zanieb reviewed on 2023-11-27 16:29_

I think @charliermarsh  is better suited to review the implementation, but I read through everything as best as I could :)

The ecosystem changes look good.

---

_@diceroll123 reviewed on 2023-11-28 05:03_

---

_Review comment by @diceroll123 on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_list_index_lookup.py`:19 on 2023-11-28 05:03_

Adding a case for that, as well as the sequence itself being mutated! Thanks!

---

_@diceroll123 reviewed on 2023-11-28 05:42_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:222 on 2023-11-28 05:42_

Good change, I've also added detection for `builtins.enumerate` along with it, thanks!

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_list_index_lookup.py`:1 on 2023-11-30 22:35_

These tests are really thorough! Thanks for thinking through them.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:202 on 2023-11-30 22:47_

nit: using `let Some(..) = .. else { continue; }` will avoid having an additional indent

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:104 on 2023-11-30 22:52_

nit: can we avoid having nested `if` conditions by using `let Some(ast::ExprName { id, .. }) = value.as_name_expr() else { return; }`

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:147 on 2023-11-30 22:52_

Same as above

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:235 on 2023-11-30 22:54_

We could use the `call_expr.as_call_expr()?` pattern throughout this function. For example, the highlighted code would become:

```rust
let Some(ast::ExprCall { func, arguments, .. }) = call_expr.as_call_expr()?;
```

---

_@dhruvmanila approved on 2023-11-30 22:55_

Overall this is pretty good. Thanks for the thorough test cases. I just have a few nits but those shouldn't be a blocker to merge this PR.

---

_Comment by @diceroll123 on 2023-11-30 22:57_

Will address those, all good feedback, thanks @dhruvmanila!

---

_@diceroll123 reviewed on 2023-11-30 23:25_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:235 on 2023-11-30 23:25_

Looks like the right code was

```rust
let ast::ExprCall {
    func, arguments, ..
} = call_expr.as_call_expr()?;
```

That tripped me up for a sec! 🤣 Thanks for the push in the right direction though!

---

_Merged by @zanieb on 2023-11-30 23:45_

---

_Closed by @zanieb on 2023-11-30 23:45_

---

_Label `rule` added by @zanieb on 2023-11-30 23:45_

---

_Label `preview` added by @zanieb on 2023-11-30 23:45_

---

_@dhruvmanila reviewed on 2023-12-01 00:34_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:235 on 2023-12-01 00:34_

Ah right, sorry!

---
