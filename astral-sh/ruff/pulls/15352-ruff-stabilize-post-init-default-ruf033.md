```yaml
number: 15352
title: "[`ruff`] Stabilize `post-init-default` (RUF033)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: ruff-0.9
head: micha/post-init-default
created_at: 2025-01-08T13:44:11Z
updated_at: 2025-01-08T14:40:06Z
url: https://github.com/astral-sh/ruff/pull/15352
synced_at: 2026-01-10T20:34:00Z
```

# [`ruff`] Stabilize `post-init-default` (RUF033)

---

_Pull request opened by @MichaReiser on 2025-01-08 13:44_

## Summary

Stabilizes the [`post-init-default`](https://docs.astral.sh/ruff/rules/post-init-default/) rule

## Test Plan

There are no open issues or PRs related to this rule. 

There are no ecosystem changes... Makes this somewhat hard to judge.


---

_Label `rule` added by @MichaReiser on 2025-01-08 13:44_

---

_Comment by @github-actions[bot] on 2025-01-08 13:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/5d58b1fa7822fb1b8ff28cacc8a3c37e521e5cc3/testing/_py/test_local.py#L21'>testing/_py/test_local.py:21:12:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI006 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/a2492dde3fedb1fac6a5b23c38ad0100874f69b3/airflow/decorators/condition.py#L32'>airflow/decorators/condition.py:32:35:</a> TC008 [*] Remove quotes from type alias
- <a href='https://github.com/apache/airflow/blob/a2492dde3fedb1fac6a5b23c38ad0100874f69b3/airflow/decorators/condition.py#L33'>airflow/decorators/condition.py:33:35:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/ldata/_transfer/upload.py#L30'>src/latch/ldata/_transfer/upload.py:30:32:</a> TC008 [*] Remove quotes from type alias
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/ldata/_transfer/upload.py#L31'>src/latch/ldata/_transfer/upload.py:31:35:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC008 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2025-01-08 13:52_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-08 13:57_

---

_@AlexWaygood approved on 2025-01-08 14:24_

---

_Merged by @MichaReiser on 2025-01-08 14:40_

---

_Closed by @MichaReiser on 2025-01-08 14:40_

---

_Branch deleted on 2025-01-08 14:40_

---
