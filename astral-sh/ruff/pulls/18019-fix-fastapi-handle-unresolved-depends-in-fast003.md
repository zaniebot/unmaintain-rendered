```yaml
number: 18019
title: "fix(fastapi): Handle unresolved Depends in FAST003"
type: pull_request
state: closed
author: danparizher
labels: []
assignees: []
base: main
head: fix-fast003-unresolved-depends
created_at: 2025-05-11T19:33:21Z
updated_at: 2025-05-12T07:01:10Z
url: https://github.com/astral-sh/ruff/pull/18019
synced_at: 2026-01-10T18:57:03Z
```

# fix(fastapi): Handle unresolved Depends in FAST003

---

_Pull request opened by @danparizher on 2025-05-11 19:33_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

DISCLAIMER! This is one of my first times contributing to this repository so I am sure I am missing some steps. I would appreciate any guidance and feedback!

---

This PR fixes an issue where the `FAST003` rule (`FastApiUnusedPathParameter`) would prematurely stop checking for missing path parameters if it encountered a `fastapi.Depends` with a dependency that could not be fully resolved (e.g., an imported function from an external, unanalyzed module).

The original behavior caused the rule to miss genuinely missing path parameters if an unresolvable dependency appeared earlier in the function signature's parameter list.

This change modifies the rule to ensure that it continues to process all declared path parameters against the main function's direct signature and any resolvable `Depends` parameters, even if some other `Depends` targets are unresolvable.

Fixes #18009

## Test Plan

1.  **New Test Case for External Dependencies**:
    *   Added `crates/ruff_linter/resources/test/fixtures/fastapi/FAST003_external_dependency.py` which specifically replicates the scenario described in the issue, where an unresolvable `Depends` is present alongside a missing path parameter.
    *   Added a helper module `crates/ruff_linter/resources/test/fixtures/fastapi/external_module.py` for this test.
    *   The corresponding snapshot test (`ruff_linter__rules__fastapi__tests__fast-api-unused-path-parameter_FAST003_external_dependency.py.snap`) was generated and verified to correctly identify the missing path parameter despite the unresolvable dependency.

2.  **Existing Test Case Update**:
    *   The existing snapshot for `FAST003.py` (`ruff_linter__rules__fastapi__tests__fast-api-unused-path-parameter_FAST003.py.snap`) was updated. The changes show that the rule now correctly identifies additional missing path parameters in scenarios with complex local dependencies that might have previously caused an early exit. This indicates improved thoroughness of the rule.

3.  **Manual Verification**:
    *   The example code provided in issue #18009 was saved to an example file.
    *   Ran `cargo run -p ruff -- check example.py --select FAST003 --no-cache`.
    *   Verified that the command now correctly reports the `FAST003` violation for the missing `user_id`, as expected.

4.  **Pre-commit checks**:
    *   `cargo test -p ruff_linter` was run, and snapshot tests were updated and pass.
    *   `uvx pre-commit run --all-files --show-diff-on-failure` was run. `cargo fmt` applied necessary formatting changes, which are included in this PR. Other pre-commit checks passed.
    *   `cargo clippy --workspace --all-targets --all-features -- -D warnings` was run.


---

_Review requested from @carljm by @danparizher on 2025-05-11 19:33_

---

_Review requested from @AlexWaygood by @danparizher on 2025-05-11 19:33_

---

_Review requested from @sharkdp by @danparizher on 2025-05-11 19:33_

---

_Review requested from @dcreager by @danparizher on 2025-05-11 19:33_

---

_Comment by @github-actions[bot] on 2025-05-11 19:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-12 00:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py:91:16:</a> FAST003 Parameter `run_id` appears in route path, but not in `head_xcom` signature
+ <a href='https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py:91:25:</a> FAST003 Parameter `task_id` appears in route path, but not in `head_xcom` signature
+ <a href='https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py:91:35:</a> FAST003 Parameter `key` appears in route path, but not in `head_xcom` signature
+ <a href='https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py:91:7:</a> FAST003 Parameter `dag_id` appears in route path, but not in `head_xcom` signature
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FAST003 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py:91:16:</a> FAST003 Parameter `run_id` appears in route path, but not in `head_xcom` signature
+ <a href='https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py:91:25:</a> FAST003 Parameter `task_id` appears in route path, but not in `head_xcom` signature
+ <a href='https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py:91:35:</a> FAST003 Parameter `key` appears in route path, but not in `head_xcom` signature
+ <a href='https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L91'>airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py:91:7:</a> FAST003 Parameter `dag_id` appears in route path, but not in `head_xcom` signature
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FAST003 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Review request for @dcreager removed by @AlexWaygood on 2025-05-12 00:57_

---

_Review request for @carljm removed by @AlexWaygood on 2025-05-12 00:57_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-05-12 00:57_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-12 00:57_

---

_Comment by @MichaReiser on 2025-05-12 07:00_

Thanks for your contribution. 

Unfortunately, we can't make this change because it will trade false-negatives with false-positives. You can see this by the ecosystem results: All airflow reports are now false positives because all missing parameters are actually handled the the shared function [`xcom_query`](https://github.com/apache/airflow/blob/323cfb6e95fa52149b1cd804b7454e19019048f4/airflow-core/src/airflow/api_fastapi/execution_api/routes/xcoms.py#L71-L87)

---

_Closed by @MichaReiser on 2025-05-12 07:00_

---
