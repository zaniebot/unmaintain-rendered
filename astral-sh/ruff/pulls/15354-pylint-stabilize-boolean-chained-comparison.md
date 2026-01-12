```yaml
number: 15354
title: "[`pylint`]: Stabilize `boolean-chained-comparison` (`PLR1716`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: ruff-0.9
head: micha/boolean-chain-comparison
created_at: 2025-01-08T14:04:22Z
updated_at: 2025-01-08T14:43:04Z
url: https://github.com/astral-sh/ruff/pull/15354
synced_at: 2026-01-12T15:55:51Z
```

# [`pylint`]: Stabilize `boolean-chained-comparison` (`PLR1716`)

---

_@MichaReiser_

## Summary

Stabilizes [`boolean-chained-comparison`](https://docs.astral.sh/ruff/rules/boolean-chained-comparison/). 


## Test Plan

There are no open issues or PRs related to this rule.

The last bug fix was implemented about a month ago.


---

_Label `rule` added by @MichaReiser on 2025-01-08 14:06_

---

_Added to milestone `v0.9` by @MichaReiser on 2025-01-08 14:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-08 14:10_

---

_Comment by @github-actions[bot] on 2025-01-08 14:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/5d58b1fa7822fb1b8ff28cacc8a3c37e521e5cc3/testing/_py/test_local.py#L21'>testing/_py/test_local.py:21:12:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/cosmology/flrw/lambdacdm.py#L267'>astropy/cosmology/flrw/lambdacdm.py:267:15:</a> PLR1716 [*] Contains chained boolean comparison that can be simplified
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI006 | 1 | 1 | 0 | 0 | 0 |
| PLR1716 | 1 | 1 | 0 | 0 | 0 |

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

_Marked ready for review by @MichaReiser on 2025-01-08 14:11_

---

_@AlexWaygood approved on 2025-01-08 14:25_

---

_Merged by @MichaReiser on 2025-01-08 14:43_

---

_Closed by @MichaReiser on 2025-01-08 14:43_

---

_Branch deleted on 2025-01-08 14:43_

---
