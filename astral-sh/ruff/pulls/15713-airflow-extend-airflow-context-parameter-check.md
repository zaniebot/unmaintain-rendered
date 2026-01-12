```yaml
number: 15713
title: "[`airflow`] Extend airflow context parameter check for `BaseOperator.execute` (`AIR302`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-airflow-context-check
created_at: 2025-01-24T10:45:39Z
updated_at: 2025-02-03T05:48:40Z
url: https://github.com/astral-sh/ruff/pull/15713
synced_at: 2026-01-12T15:55:52Z
```

# [`airflow`] Extend airflow context parameter check for `BaseOperator.execute` (`AIR302`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
* feat
    * add is_execute_method_inherits_from_airflow_operator for checking the removed context key in the execute method
* refactor: rename
    * is_airflow_task as is_airflow_task_function_def
    * in_airflow_task as in_airflow_task_function_def
    * removed_in_3 as airflow_3_removal_expr
    * removed_in_3_function_def as airflow_3_removal_function_def
* test:
    * reorganize test cases

## Test Plan

<!-- How was it tested? -->
a test fixture has been updated

---

_Comment by @Lee-W on 2025-01-24 10:49_

as a heads up, I'll be less active next week as it's the lunar new year in Taiwan. Thanks for helping out!

---

_Comment by @github-actions[bot] on 2025-01-24 10:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2025-01-24 13:51_

> refactor: rename
> * is_airflow_task as is_decorated_by_airflow_task
> * check_function_parameters as check_parameters_in_function_def
> * removed_in_3 as removed_expr_in_3
> * removed_in_3_function_def as removed_funciton_def_in_3

Can you provide the motivation for these renames? We usually prefer to have active voice instead of passive (at least regarding the first two renames) (https://github.com/astral-sh/ruff/blob/2b3550c85fcc55d93216c4d4ef64c5f0a5766053/crates/ruff_python_semantic/src/analyze/visibility.rs). I'm a bit torn on the last two renames because it indicates to me that some expression / function definition has been removed but that's not the case. I think we should rename them to `airflow_3_removal` (similar to [`numpy_2_0_deprecation`](https://github.com/astral-sh/ruff/blob/2b3550c85fcc55d93216c4d4ef64c5f0a5766053/crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs#L158-L159)) and add suffix `_expr` / `_function_def` to indicate the node that's being checked. Happy to hear your thoughts on this.

---

_Comment by @dhruvmanila on 2025-01-24 13:52_

> as a heads up, I'll be less active next week as it's the lunar new year in Taiwan. Thanks for helping out!

Thanks for the heads up. Have a great week! Do you know if there's more work required to complete https://github.com/astral-sh/ruff/issues/14626 ?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1115 on 2025-01-24 14:02_

```suggestion
    class_def.bases().iter().any(|class_base| {
        semantic
            .resolve_qualified_name(class_base)
            .is_some_and(|qualified_name| {
                matches!(qualified_name.segments(), ["airflow", .., "BaseOperator"])
            })
    })
```

---

_@dhruvmanila approved on 2025-01-24 14:03_

Looks good, thank you.

I've a minor concern about the renames.

---

_Label `rule` added by @dhruvmanila on 2025-01-24 14:03_

---

_Label `preview` added by @dhruvmanila on 2025-01-24 14:03_

---

_Comment by @Lee-W on 2025-01-24 15:13_

> > refactor: rename
> > 
> > * is_airflow_task as is_decorated_by_airflow_task
> > * check_function_parameters as check_parameters_in_function_def
> > * removed_in_3 as removed_expr_in_3
> > * removed_in_3_function_def as removed_funciton_def_in_3
> 
> Can you provide the motivation for these renames? We usually prefer to have active voice instead of passive (at least regarding the first two renames) (https://github.com/astral-sh/ruff/blob/2b3550c85fcc55d93216c4d4ef64c5f0a5766053/crates/ruff_python_semantic/src/analyze/visibility.rs). I'm a bit torn on the last two renames because it indicates to me that some expression / function definition has been removed but that's not the case. I think we should rename them to `airflow_3_removal` (similar to [`numpy_2_0_deprecation`](https://github.com/astral-sh/ruff/blob/2b3550c85fcc55d93216c4d4ef64c5f0a5766053/crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs#L158-L159)) and add suffix `_expr` / `_function_def` to indicate the node that's being checked. Happy to hear your thoughts on this.

The motivation was to make it more accurate. I tried to change it a bit. Please take a look. Thanks!

---

_Comment by @Lee-W on 2025-01-24 15:17_

> > as a heads up, I'll be less active next week as it's the lunar new year in Taiwan. Thanks for helping out!
> 
> Thanks for the heads up. Have a great week! Do you know if there's more work required to complete #14626 ?

There're 3 cases I can think of as of now.

1. `context.get("conf")` or `context["get"]` in `BaseOperator.execute`
2. In `PythonOperator(..., python_callable=func)`, the `func` needs to be checked
3. templated variable (e.g., `params="{{ conf }}"`

The first should be easy, the second might need some exploration, and the third one is a bit tricky, and I'm not even sure whether we should check it.
Whether a variable can be templated is somewhat flexible, but on the other hand, it's also uncommon to have a string happen to be in that format

---

_Review requested from @dhruvmanila by @Lee-W on 2025-01-24 16:14_

---

_Renamed from "[airflow] Extend airflow context parameter check in BaseOperator.execute (AIR302)" to "[`airflow`] Extend airflow context parameter check for `BaseOperator.execute` (`AIR302`)" by @dylwil3 on 2025-01-25 03:12_

---

_@dhruvmanila reviewed on 2025-01-27 15:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1075 on 2025-01-27 15:17_

I think we should revert these two renames as it's very confusing - the only difference between the two is "in_" and "is_ prefix.

---

_@dhruvmanila approved on 2025-01-27 15:17_

---

_@dhruvmanila reviewed on 2025-01-27 15:18_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1075 on 2025-01-27 15:18_

(I know you're out this week, so will merge this and rename as I can't edit this PR.)

---

_Merged by @dhruvmanila on 2025-01-27 15:18_

---

_Closed by @dhruvmanila on 2025-01-27 15:18_

---

_@Lee-W reviewed on 2025-02-03 05:48_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1075 on 2025-02-03 05:48_

Thanks for helping out! I tried to check whether I could allow modification, but it seems I do not have the permission either 🥲

---
