```yaml
number: 14734
title: "[airflow]: extend removed names (AIR302)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-air-302-removed-names
created_at: 2024-12-02T15:25:25Z
updated_at: 2024-12-03T21:39:43Z
url: https://github.com/astral-sh/ruff/pull/14734
synced_at: 2026-01-10T20:42:27Z
```

# [airflow]: extend removed names (AIR302)

---

_Pull request opened by @Lee-W on 2024-12-02 15:25_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Airflow 3.0 removes various deprecated functions, members, modules, and other values. They have been deprecated in 2.x, but the removal causes incompatibilities that we want to detect. This PR deprecates the following names.

* airflow.metrics.validators.AllowListValidator
* airflow.metrics.validators.BlockListValidator
* airflow.utils.dates.parse_execution_date
* airflow.utils.dates.round_time
* airflow.utils.dates.scale_time_units
* airflow.utils.dates.infer_time_unit
* airflow.utils.file.TemporaryDirectory
* airflow.utils.file.mkdirs
* airflow.www.auth.has_access
* airflow.api_connexion.security.requires_access
* airflow.utils.dag_cycle_tester.test_cycle
* airflow.utils.state.SHUTDOWN
* airflow.utils.state.terminating_states

The full list of names we will extend https://github.com/apache/airflow/issues/44556

## Test Plan

<!-- How was it tested? -->
A test fixture is included in the PR.


---

_Comment by @github-actions[bot] on 2024-12-02 15:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/edf3e3376811129a9fad278ae396b3b5644c69d8/performance/src/performance_dags/performance_dag/performance_dag.py#L244'>performance/src/performance_dags/performance_dag/performance_dag.py:244:9:</a> AIR302 `schedule_interval` is removed in Airflow 3.0; use `schedule` instead
- <a href='https://github.com/apache/airflow/blob/edf3e3376811129a9fad278ae396b3b5644c69d8/performance/src/performance_dags/performance_dag/performance_dag.py#L244'>performance/src/performance_dags/performance_dag/performance_dag.py:244:9:</a> AIR302 `schedule_interval` is removed in Airflow 3.0; use schedule instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_@uranusjr reviewed on 2024-12-03 06:37_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:117 on 2024-12-03 06:37_

These have replacements (the stdlib `tempfile`), they should be mentioned.

---

_@uranusjr reviewed on 2024-12-03 06:39_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:120 on 2024-12-03 06:39_

We should mention this can be safely removed (`apply_defaults` is now unconditionally done). It’s probably easier if we add a variant to Replacement (called Message maybe) and do

```rust
Replacement::Message(message) => {
    format!("`{deprecated}` is removed in Airflow 3.0; {message}")
}
```

in `impl Violation`.

---

_@Lee-W reviewed on 2024-12-03 06:47_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:120 on 2024-12-03 06:47_

Yep, we'll need something similar for AIR303 and AIR304 as well. or we could probably use this for those 2 🤔 

---

_@Lee-W reviewed on 2024-12-03 06:48_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:117 on 2024-12-03 06:48_

sure! will do

---

_@Lee-W reviewed on 2024-12-03 14:18_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:117 on 2024-12-03 14:18_

this is addressed

---

_@Lee-W reviewed on 2024-12-03 14:19_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:120 on 2024-12-03 14:19_

This is temporarily removed, but will be added back (probably in the next PR I think?)

---

_Renamed from "feat(air302): extend removed names" to "[airflow]: extend removed names (AIR302)" by @Lee-W on 2024-12-03 14:26_

---

_Marked ready for review by @Lee-W on 2024-12-03 14:26_

---

_Label `rule` added by @MichaReiser on 2024-12-03 21:39_

---

_Label `preview` added by @MichaReiser on 2024-12-03 21:39_

---

_Merged by @MichaReiser on 2024-12-03 21:39_

---

_Closed by @MichaReiser on 2024-12-03 21:39_

---
