```yaml
number: 17887
title: "[`airflow`] apply try catch guard to all AIR3 rules (`AIR3`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: apply-try-catch-guard-to-all-AIR3
created_at: 2025-05-06T11:22:42Z
updated_at: 2025-05-12T21:13:41Z
url: https://github.com/astral-sh/ruff/pull/17887
synced_at: 2026-01-12T15:56:07Z
```

# [`airflow`] apply try catch guard to all AIR3 rules (`AIR3`)

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

If a try-catch block guards the names, we don't raise warnings. During this change, I discovered that some of the replacement types were missed. Thus, I extend the fix to types other than AutoImport as well

## Test Plan

<!-- How was it tested? -->

Test fixtures are added and updated.

---

_Comment by @github-actions[bot] on 2025-05-06 11:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -33 violations, +270 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -33 violations, +270 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/baseoperator.py#L301'>airflow-core/src/airflow/models/baseoperator.py:301:26:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/baseoperator.py#L301'>airflow-core/src/airflow/models/baseoperator.py:301:26:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/baseoperator.py#L305'>airflow-core/src/airflow/models/baseoperator.py:305:28:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/baseoperator.py#L305'>airflow-core/src/airflow/models/baseoperator.py:305:28:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/baseoperator.py#L382'>airflow-core/src/airflow/models/baseoperator.py:382:41:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/baseoperator.py#L382'>airflow-core/src/airflow/models/baseoperator.py:382:41:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/baseoperator.py#L433'>airflow-core/src/airflow/models/baseoperator.py:433:41:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/baseoperator.py#L433'>airflow-core/src/airflow/models/baseoperator.py:433:41:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagbag.py#L474'>airflow-core/src/airflow/models/dagbag.py:474:95:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagbag.py#L474'>airflow-core/src/airflow/models/dagbag.py:474:95:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L1261'>airflow-core/src/airflow/models/dagrun.py:1261:50:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L1261'>airflow-core/src/airflow/models/dagrun.py:1261:50:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L1575'>airflow-core/src/airflow/models/dagrun.py:1575:20:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L1575'>airflow-core/src/airflow/models/dagrun.py:1575:20:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L1975'>airflow-core/src/airflow/models/dagrun.py:1975:36:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L1975'>airflow-core/src/airflow/models/dagrun.py:1975:36:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L206'>airflow-core/src/airflow/models/dagrun.py:206:14:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L206'>airflow-core/src/airflow/models/dagrun.py:206:14:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L208'>airflow-core/src/airflow/models/dagrun.py:208:14:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L208'>airflow-core/src/airflow/models/dagrun.py:208:14:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L861'>airflow-core/src/airflow/models/dagrun.py:861:26:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/dagrun.py#L861'>airflow-core/src/airflow/models/dagrun.py:861:26:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/taskmap.py#L172'>airflow-core/src/airflow/models/taskmap.py:172:65:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/taskmap.py#L172'>airflow-core/src/airflow/models/taskmap.py:172:65:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/xcom_arg.py#L100'>airflow-core/src/airflow/models/xcom_arg.py:100:54:</a> AIR311 [*] `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/src/airflow/models/xcom_arg.py#L100'>airflow-core/src/airflow/models/xcom_arg.py:100:54:</a> AIR311 `airflow.models.dag.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 273 additional changes omitted for rule AIR311
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/tests/unit/utils/test_dot_renderer.py#L103'>airflow-core/tests/unit/utils/test_dot_renderer.py:103:22:</a> AIR312 `airflow.operators.python.PythonOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/airflow-core/tests/unit/utils/test_dot_renderer.py#L83'>airflow-core/tests/unit/utils/test_dot_renderer.py:83:22:</a> AIR312 `airflow.operators.python.PythonOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py#L129'>providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py:129:24:</a> AIR312 `airflow.operators.bash.BashOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py#L133'>providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py:133:17:</a> AIR312 `airflow.operators.empty.EmptyOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py#L138'>providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py:138:22:</a> AIR312 `airflow.operators.python.PythonOperator` is deprecated and moved into `standard` provider in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 272 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 298 | 0 | 28 | 270 | 0 |
| AIR312 | 5 | 0 | 5 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2025-05-07 06:36_

---

_Label `preview` added by @MichaReiser on 2025-05-07 06:36_

---

_Comment by @MichaReiser on 2025-05-07 06:37_

Can you explain to me why this pr increases the number of fixes for AIR311? Does this PR contain an other change that should be mentioned in the PR summary.

---

_Converted to draft by @Lee-W on 2025-05-07 07:48_

---

_Comment by @Lee-W on 2025-05-07 07:53_

> Can you explain to me why this pr increases the number of fixes for AIR311? Does this PR contain an other change that should be mentioned in the PR summary.

Let me take another look. I just mark this one as draft

---

_Comment by @Lee-W on 2025-05-07 13:06_

Just found the reason. Some replacement types were missed in the previous version. Let me update the description.

---

_Marked ready for review by @Lee-W on 2025-05-07 13:06_

---

_Review requested from @ntBre by @MichaReiser on 2025-05-09 06:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:960 on 2025-05-09 15:40_

It's a bit unfortunate to duplicate these blocks so many times, but I don't see a super easy way to factor out a helper method. One idea is to extract `module` and `name` first to avoid duplicating the `try_set_fix` calls at least:

```suggestion
    let semantic = checker.semantic();
    if let Some((module, name)) = match &replacement {
        ProviderReplacement::AutoImport { module, name, .. } => Some((module, *name)),
        ProviderReplacement::SourceModuleMovedToProvider { module, name, .. } => {
            Some((module, name.as_str()))
        }
        _ => None,
    } {
        if is_guarded_by_try_except(expr, module, name, semantic) {
            return;
        }
        diagnostic.try_set_fix(|| {
            let (import_edit, binding) = checker.importer().get_or_import_symbol(
                &ImportRequest::import_from(module, name),
                expr.start(),
                checker.semantic(),
            )?;
            let replacement_edit = Edit::range_replacement(binding, ranged.range());
            Ok(Fix::safe_edits(import_edit, [replacement_edit]))
        });
    }
```

---

_@ntBre approved on 2025-05-09 15:43_

Looks good, just one suggestion that might not be worth handling.

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:960 on 2025-05-12 10:50_

Yep, I tried a few ways but failed. Thanks for the suggestion. I just updated it to all the rules!

---

_@Lee-W reviewed on 2025-05-12 10:50_

---

_@ntBre approved on 2025-05-12 21:13_

---

_Merged by @ntBre on 2025-05-12 21:13_

---

_Closed by @ntBre on 2025-05-12 21:13_

---
