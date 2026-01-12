```yaml
number: 10212
title: "[`refurb`] Implement `list_assign_reversed` lint (FURB187)"
type: pull_request
state: merged
author: alex-700
labels:
  - rule
assignees: []
merged: true
base: main
head: latyshev/furb-187
created_at: 2024-03-03T18:04:18Z
updated_at: 2024-03-22T11:03:03Z
url: https://github.com/astral-sh/ruff/pull/10212
synced_at: 2026-01-12T15:55:31Z
```

# [`refurb`] Implement `list_assign_reversed` lint (FURB187)

---

_@alex-700_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Implement [use_reverse (FURB187)](https://github.com/dosisod/refurb/blob/master/refurb/checks/readability/use_reverse.py) lint.
Tests were copied from original https://github.com/dosisod/refurb/blob/master/test/data/err_187.py.

## Test Plan

<!-- How was it tested? -->
cargo test


---

_Comment by @github-actions[bot] on 2024-03-03 18:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6fa7b726d6fcec6dd6fbf844a08d3fcff9e06711/airflow/jobs/backfill_job_runner.py#L901'>airflow/jobs/backfill_job_runner.py:901:13:</a> FURB187 [*] Use of assignment of `reversed` on list `dagrun_infos`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB187 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-03-04 10:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-09 00:01_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-09 00:01_

---

_@charliermarsh approved on 2024-03-21 16:58_

Thanks! Sorry for the delay.

---

_Merged by @charliermarsh on 2024-03-21 17:09_

---

_Closed by @charliermarsh on 2024-03-21 17:09_

---

_Branch deleted on 2024-03-22 11:03_

---
