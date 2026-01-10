```yaml
number: 19239
title: "[airflow] stablize AIR302"
type: pull_request
state: closed
author: CodeMan62
labels:
  - breaking
assignees: []
draft: true
base: main
head: codeman/17749
created_at: 2025-07-09T17:07:54Z
updated_at: 2025-09-05T14:10:32Z
url: https://github.com/astral-sh/ruff/pull/19239
synced_at: 2026-01-10T17:46:21Z
```

# [airflow] stablize AIR302

---

_Pull request opened by @CodeMan62 on 2025-07-09 17:07_

## Summary

#17749 

## Test Plan
testing mentioned in contributing.md .


---

_Added to milestone `v0.13` by @MichaReiser on 2025-07-09 17:08_

---

_Label `breaking` added by @MichaReiser on 2025-07-09 17:08_

---

_Comment by @github-actions[bot] on 2025-07-09 17:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f98bf389f06687abbae581f7d61c175003e0ddb9/dev/airflow_perf/dags/perf_dag_1.py#L41'>dev/airflow_perf/dags/perf_dag_1.py:41:10:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/f98bf389f06687abbae581f7d61c175003e0ddb9/dev/airflow_perf/dags/perf_dag_1.py#L48'>dev/airflow_perf/dags/perf_dag_1.py:48:12:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is moved into `standard` provider in Airflow 3.0;
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-07-09 20:17_

Thanks for this pr. I suggest that we wait with other stabilization PRs until we're closer to shipping the next minor because we can't merge those changes now.

---

_Comment by @CodeMan62 on 2025-07-10 01:38_

make sense. I have stablized almost all the rules mentioned in 0.13 roadmap if you want you can mention the rules/issues here that needs to stablize i would love to work on then 

---

_Converted to draft by @dhruvmanila on 2025-07-10 06:20_

---

_Comment by @dhruvmanila on 2025-07-10 06:22_

Thank you for putting up all of the PRs! 

Since it's going to be sometime since we start working on 0.13 as we released 0.12 a month ago, I'll be putting all of these PRs into draft so that it's easier for us, the maintainers, to prioritize the PRs that needs to be reviewed. Hope you don't mind.

---

_Comment by @ntBre on 2025-09-05 14:10_

I'll close this in favor of https://github.com/astral-sh/ruff/pull/20250, but thank you! I've merged several of your other PRs into the release branch.

---

_Closed by @ntBre on 2025-09-05 14:10_

---
