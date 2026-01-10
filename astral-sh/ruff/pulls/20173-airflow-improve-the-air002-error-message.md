```yaml
number: 20173
title: "[`airflow`] Improve the `AIR002` error message"
type: pull_request
state: merged
author: Lee-W
labels:
  - preview
  - diagnostics
assignees: []
merged: true
base: main
head: improve-air-message
created_at: 2025-08-31T01:33:26Z
updated_at: 2025-09-04T09:59:16Z
url: https://github.com/astral-sh/ruff/pull/20173
synced_at: 2026-01-10T17:46:21Z
```

# [`airflow`] Improve the `AIR002` error message

---

_Pull request opened by @Lee-W on 2025-08-31 01:33_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->


### What
Change the message from "DAG should have an explicit `schedule` argument" to "`DAG` or `@dag` should have an explicit `schedule` argument"

### Why
We're trying to get rid of the idea that DAG in airflow was Directed acyclic graph. Thus, change it to refer to the class `DAG` or the decorator `@dag` might help a bit.

## Test Plan

<!-- How was it tested? -->

update the test fixtures accordly


---

_Renamed from "docs(AIR002): improve AIR002 error message" to "[`airflow`] improve AIR002 error message (`AIR002`)" by @Lee-W on 2025-08-31 01:43_

---

_Comment by @github-actions[bot] on 2025-08-31 01:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+203 -203 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+203 -203 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/src/airflow/models/dag.py#L288'>airflow-core/src/airflow/models/dag.py:288:16:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/src/airflow/models/dag.py#L288'>airflow-core/src/airflow/models/dag.py:288:16:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_xcom.py#L406'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_xcom.py:406:15:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_xcom.py#L406'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_xcom.py:406:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L362'>airflow-core/tests/unit/dag_processing/test_collection.py:362:15:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L362'>airflow-core/tests/unit/dag_processing/test_collection.py:362:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L446'>airflow-core/tests/unit/dag_processing/test_collection.py:446:15:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L446'>airflow-core/tests/unit/dag_processing/test_collection.py:446:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L465'>airflow-core/tests/unit/dag_processing/test_collection.py:465:15:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L465'>airflow-core/tests/unit/dag_processing/test_collection.py:465:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L516'>airflow-core/tests/unit/dag_processing/test_collection.py:516:15:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L516'>airflow-core/tests/unit/dag_processing/test_collection.py:516:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L677'>airflow-core/tests/unit/dag_processing/test_collection.py:677:15:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L677'>airflow-core/tests/unit/dag_processing/test_collection.py:677:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L712'>airflow-core/tests/unit/dag_processing/test_collection.py:712:15:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_collection.py#L712'>airflow-core/tests/unit/dag_processing/test_collection.py:712:15:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L1004'>airflow-core/tests/unit/dag_processing/test_processor.py:1004:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L1004'>airflow-core/tests/unit/dag_processing/test_processor.py:1004:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L1052'>airflow-core/tests/unit/dag_processing/test_processor.py:1052:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L1052'>airflow-core/tests/unit/dag_processing/test_processor.py:1052:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L134'>airflow-core/tests/unit/dag_processing/test_processor.py:134:18:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L134'>airflow-core/tests/unit/dag_processing/test_processor.py:134:18:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L171'>airflow-core/tests/unit/dag_processing/test_processor.py:171:18:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L171'>airflow-core/tests/unit/dag_processing/test_processor.py:171:18:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L204'>airflow-core/tests/unit/dag_processing/test_processor.py:204:18:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L204'>airflow-core/tests/unit/dag_processing/test_processor.py:204:18:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L247'>airflow-core/tests/unit/dag_processing/test_processor.py:247:18:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L247'>airflow-core/tests/unit/dag_processing/test_processor.py:247:18:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L282'>airflow-core/tests/unit/dag_processing/test_processor.py:282:18:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L282'>airflow-core/tests/unit/dag_processing/test_processor.py:282:18:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L313'>airflow-core/tests/unit/dag_processing/test_processor.py:313:18:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L313'>airflow-core/tests/unit/dag_processing/test_processor.py:313:18:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L508'>airflow-core/tests/unit/dag_processing/test_processor.py:508:11:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L508'>airflow-core/tests/unit/dag_processing/test_processor.py:508:11:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L540'>airflow-core/tests/unit/dag_processing/test_processor.py:540:10:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L540'>airflow-core/tests/unit/dag_processing/test_processor.py:540:10:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L589'>airflow-core/tests/unit/dag_processing/test_processor.py:589:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L589'>airflow-core/tests/unit/dag_processing/test_processor.py:589:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L662'>airflow-core/tests/unit/dag_processing/test_processor.py:662:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L662'>airflow-core/tests/unit/dag_processing/test_processor.py:662:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L707'>airflow-core/tests/unit/dag_processing/test_processor.py:707:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L707'>airflow-core/tests/unit/dag_processing/test_processor.py:707:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L777'>airflow-core/tests/unit/dag_processing/test_processor.py:777:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L777'>airflow-core/tests/unit/dag_processing/test_processor.py:777:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L805'>airflow-core/tests/unit/dag_processing/test_processor.py:805:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L805'>airflow-core/tests/unit/dag_processing/test_processor.py:805:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L846'>airflow-core/tests/unit/dag_processing/test_processor.py:846:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L846'>airflow-core/tests/unit/dag_processing/test_processor.py:846:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
- <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L893'>airflow-core/tests/unit/dag_processing/test_processor.py:893:14:</a> AIR002 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a811a586a3ffa546af16fd2b75de410d9b132fc7/airflow-core/tests/unit/dag_processing/test_processor.py#L893'>airflow-core/tests/unit/dag_processing/test_processor.py:893:14:</a> AIR002 `DAG` or `@dag` should have an explicit `schedule` argument
... 356 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR002 | 406 | 203 | 203 | 0 | 0 |

</p>
</details>




---

_@ntBre approved on 2025-09-03 13:17_

Thank you!

A bit unrelated to this PR, but we're getting ready for the 0.13 release early next week. We're planning to stabilize AIR002, AIR301, AIR302, AIR311, and AIR312. Does that sound good to you?

The only reason to hold off is if you plan to make substantial changes to any of these rules, in which case you might prefer to keep the whole rule in preview.

I was going to ping you on the stabilization PR itself but wanted to give you a heads-up!

I'll review https://github.com/astral-sh/ruff/pull/20202 and https://github.com/astral-sh/ruff/pull/20172, so they can be part of the stable release too.

---

_Label `preview` added by @ntBre on 2025-09-03 13:22_

---

_Label `diagnostics` added by @ntBre on 2025-09-03 13:22_

---

_Renamed from "[`airflow`] improve AIR002 error message (`AIR002`)" to "[`airflow`] Improve the `AIR002` error message" by @ntBre on 2025-09-03 13:22_

---

_Merged by @ntBre on 2025-09-03 13:22_

---

_Closed by @ntBre on 2025-09-03 13:22_

---

_Comment by @Lee-W on 2025-09-03 13:23_

> Thank you!
> 
> A bit unrelated to this PR, but we're getting ready for the 0.13 release early next week. We're planning to stabilize AIR002, AIR301, AIR302, AIR311, and AIR312. Does that sound good to you?
> 
> The only reason to hold off is if you plan to make substantial changes to any of these rules, in which case you might prefer to keep the whole rule in preview.
> 
> I was going to ping you on the stabilization PR itself but wanted to give you a heads-up!
> 
> I'll review #20202 and #20172, so they can be part of the stable release too.

We might extend some more rules, but I don't expect there will be a removal for the time being. would it be ok in this case?

---

_Comment by @ntBre on 2025-09-03 13:34_

By "extend" do you mean changes like the two PRs I linked? If so, I think that would be okay. Larger changes _can_ be okay, but we'll likely have to preview-gate them in line with our [versioning guidelines](https://docs.astral.sh/ruff/versioning/#version-changes).

---

_Comment by @Lee-W on 2025-09-03 13:39_

Yes, something like these 2 PRs. 

The following 2 issues are the ones we might extend AIR301 but not yet have the bandwidth to actually do it (the second one is easier, I might be able to create a PR tomorrow)

https://github.com/apache/airflow/issues/41641
https://github.com/apache/airflow/issues/55208



---

_Comment by @ntBre on 2025-09-03 13:54_

I think that should be okay then, thank you!

---

_Comment by @Lee-W on 2025-09-04 09:59_

Thanks!

---
