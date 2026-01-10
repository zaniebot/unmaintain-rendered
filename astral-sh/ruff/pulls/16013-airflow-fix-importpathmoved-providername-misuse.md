```yaml
number: 16013
title: "[`airflow`] Fix `ImportPathMoved` / `ProviderName` misuse (`AIR303`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-AIR303-misuse
created_at: 2025-02-07T07:29:48Z
updated_at: 2025-02-12T07:05:41Z
url: https://github.com/astral-sh/ruff/pull/16013
synced_at: 2026-01-10T19:57:22Z
```

# [`airflow`] Fix `ImportPathMoved` / `ProviderName` misuse (`AIR303`)

---

_Pull request opened by @Lee-W on 2025-02-07 07:29_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->


* fix ImportPathMoved / ProviderName misuse
    * oncrete names, such as `["airflow", "config_templates", "default_celery", "DEFAULT_CELERY_CONFIG"]`, should use `ProviderName`. In contrast, module paths like `"airflow", "operators", "weekday", ...` should use `ImportPathMoved`. Misuse may lead to incorrect detection.

## Test Plan

<!-- How was it tested? -->

update test fixture


---

_Comment by @Lee-W on 2025-02-07 07:36_

I'm thinking of splitting related test cases into separate files in the following PRs. Otherwise, it's a bit difficult to check the test cases.

---

_Comment by @github-actions[bot] on 2025-02-07 07:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/dev/perf/dags/perf_dag_1.py#L41'>dev/perf/dags/perf_dag_1.py:41:10:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/dev/perf/dags/perf_dag_1.py#L41'>dev/perf/dags/perf_dag_1.py:41:10:</a> AIR303 Import path `airflow.operators.bash_operator` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/dev/perf/dags/perf_dag_1.py#L48'>dev/perf/dags/perf_dag_1.py:48:12:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/dev/perf/dags/perf_dag_1.py#L48'>dev/perf/dags/perf_dag_1.py:48:12:</a> AIR303 Import path `airflow.operators.bash_operator` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR302 `airflow.operators.bash.BashOperator` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR303 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L115'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:115:24:</a> AIR302 `airflow.operators.bash.BashOperator` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L115'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:115:24:</a> AIR303 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/tests/decorators/test_bash.py#L521'>tests/decorators/test_bash.py:521:13:</a> AIR302 `airflow.operators.bash.BashOperator` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/dea2cc9afc61caf49621c3b1923bcf90e96e17e9/tests/decorators/test_bash.py#L521'>tests/decorators/test_bash.py:521:13:</a> AIR303 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR303 | 5 | 5 | 0 | 0 | 0 |
| AIR302 | 5 | 0 | 5 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @dhruvmanila on 2025-02-07 08:48_

---

_Label `preview` added by @dhruvmanila on 2025-02-07 08:48_

---

_Comment by @dhruvmanila on 2025-02-07 08:50_

> I'm thinking of splitting related test cases into separate files in the following PRs. Otherwise, it's a bit difficult to check the test cases.

I think that'd be useful. We usually prefer self-contained PRs as it's much easier to review. As per the PR description, it seems like this might be doing multiple things (Fix `AIR303`, refactor `AIR302` and improve error message in `AIR302`). If it's not too much to ask, is it possible to split them in their own separate PRs? It'll also be useful for the CHANGELOG as the entries are generated using PR title. Let me know what you prefer, otherwise I can review the PR.

---

_Comment by @Lee-W on 2025-02-07 09:02_

> > I'm thinking of splitting related test cases into separate files in the following PRs. Otherwise, it's a bit difficult to check the test cases.
> 
> I think that'd be useful. We usually prefer self-contained PRs as it's much easier to review. As per the PR description, it seems like this might be doing multiple things (Fix `AIR303`, refactor `AIR302` and improve error message in `AIR302`). If it's not too much to ask, is it possible to split them in their own separate PRs? It'll also be useful for the CHANGELOG as the entries are generated using PR title. Let me know what you prefer, otherwise I can review the PR.

I love this idea! I was worried about creating too many small PRs. Thanks for letting me know!

---

_Converted to draft by @Lee-W on 2025-02-07 09:03_

---

_Marked ready for review by @Lee-W on 2025-02-07 10:50_

---

_Comment by @Lee-W on 2025-02-07 10:51_

I just splited this into 3 pull requests (including this one). 🙌 I may need to rebase a bit after one is merged, but I should be fine.

---

_Comment by @dhruvmanila on 2025-02-11 09:34_

@Lee-W Is this ready to be reviewed? Is it rebased after the split PRs have been merged?

---

_Comment by @Lee-W on 2025-02-11 10:31_

> @Lee-W Is this ready to be reviewed? Is it rebased after the split PRs have been merged?

Hi, yes, this is already based on those PRs. 

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:911 on 2025-02-12 05:57_

Should the `new_path` here be `bash_operator` (similar to the change above in `bash`) ?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:592 on 2025-02-12 05:58_

nit: Is this how `rustfmt` is formatting this with `name` field being indented more than the other fields? Seems like a bug if that's the case.

---

_@dhruvmanila approved on 2025-02-12 05:59_

Looks good, minor one small question.

---

_@Lee-W reviewed on 2025-02-12 06:57_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:911 on 2025-02-12 06:57_

No, it should be `bash`. we move all these things to `airflow.providers.standard.operators.bash`

---

_@Lee-W reviewed on 2025-02-12 06:57_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:592 on 2025-02-12 06:57_

hmmm... didn't notice this one. let me check again

---

_@Lee-W reviewed on 2025-02-12 06:59_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:592 on 2025-02-12 06:59_

I guess this was just my mistake. just fixed it. thanks!

---

_Renamed from "[airflow] fix ImportPathMoved / ProviderName misuse (AIR303)" to "[`airflow`] Fix `ImportPathMoved` / `ProviderName` misuse (`AIR303`)" by @dhruvmanila on 2025-02-12 07:03_

---

_Merged by @dhruvmanila on 2025-02-12 07:04_

---

_Closed by @dhruvmanila on 2025-02-12 07:04_

---
