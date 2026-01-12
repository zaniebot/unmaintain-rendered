```yaml
number: 18564
title: "Stabilize `unnecessary-empty-iterable-within-deque-call` (`RUF037`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-ruf037
created_at: 2025-06-08T19:16:13Z
updated_at: 2025-06-09T18:51:39Z
url: https://github.com/astral-sh/ruff/pull/18564
synced_at: 2026-01-12T15:56:21Z
```

# Stabilize `unnecessary-empty-iterable-within-deque-call` (`RUF037`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:16_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:16_

---

_Comment by @github-actions[bot] on 2025-06-08 19:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/training/test_interactive.py#L663'>tests/core/training/test_interactive.py:663:58:</a> RUF037 [*] Unnecessary empty iterable within a deque call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5942'>airflow-core/tests/unit/jobs/test_scheduler_job.py:5942:38:</a> RUF037 [*] Unnecessary empty iterable within a deque call
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/jobs/test_scheduler_job.py#L5943'>airflow-core/tests/unit/jobs/test_scheduler_job.py:5943:43:</a> RUF037 [*] Unnecessary empty iterable within a deque call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF037 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:28_

---

_@ntBre approved on 2025-06-09 18:48_

---

_Merged by @dylwil3 on 2025-06-09 18:51_

---

_Closed by @dylwil3 on 2025-06-09 18:51_

---

_Branch deleted on 2025-06-09 18:51_

---
