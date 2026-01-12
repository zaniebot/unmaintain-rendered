```yaml
number: 15351
title: "[`ruff`] Stabilize `useless-if-else` (`RUF034`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: ruff-0.9
head: micha/useless-if-else
created_at: 2025-01-08T13:23:26Z
updated_at: 2025-01-08T14:56:26Z
url: https://github.com/astral-sh/ruff/pull/15351
synced_at: 2026-01-12T15:55:51Z
```

# [`ruff`] Stabilize `useless-if-else` (`RUF034`)

---

_@MichaReiser_

## Summary

Stabilize [`useless-if-else`](https://docs.astral.sh/ruff/rules/useless-if-else/)

I'm somewhat torn on the name:

* most other rules specific to tenary expressions use `if-expr` -> `useless-if-expr`?
* the expression itself isn't useless. It's just that the branching is useless

Clippy's name for the same rule is `if_same_then_else` which I like better because it doesn't suggest that it is useless. However, I fail to come up to address my first concern because `if-expr-if-same-then-else` sounds off but maybe it's fine?

## Test Plan

There are no open issues for this rule.

The ecosystem changes look correct to me


---

_Label `rule` added by @MichaReiser on 2025-01-08 13:23_

---

_Comment by @github-actions[bot] on 2025-01-08 13:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/utils/decorators.py#L158'>superset/utils/decorators.py:158:9:</a> RUF034 Useless `if`-`else` condition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/315b54908bd0dc8a7bc913a38fb22f03377eb6eb/pandas/tests/io/formats/style/test_style.py#L936'>pandas/tests/io/formats/style/test_style.py:936:26:</a> RUF034 Useless `if`-`else` condition
</pre>

</p>
</details>
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
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF034 | 2 | 2 | 0 | 0 | 0 |
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
- <a href='https://github.com/apache/airflow/blob/06350716d6d44c268e88d1efc33ef59ea92b018a/airflow/decorators/condition.py#L32'>airflow/decorators/condition.py:32:35:</a> TC008 [*] Remove quotes from type alias
- <a href='https://github.com/apache/airflow/blob/06350716d6d44c268e88d1efc33ef59ea92b018a/airflow/decorators/condition.py#L33'>airflow/decorators/condition.py:33:35:</a> TC008 [*] Remove quotes from type alias
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

_Marked ready for review by @MichaReiser on 2025-01-08 13:32_

---

_Renamed from "[`RUF`] Stabilize `useless-if-else` (`RUF034`)" to "[`ruff`] Stabilize `useless-if-else` (`RUF034`)" by @MichaReiser on 2025-01-08 13:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-08 13:58_

---

_Added to milestone `v0.9` by @MichaReiser on 2025-01-08 14:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/useless_if_else.rs`:14 on 2025-01-08 14:22_

Probably doesn't really need the paragraph break either -- could just have this as a second sentence of the previous paragraph!

---

_@AlexWaygood approved on 2025-01-08 14:22_

Nice! I kinda like the existing name 🤷

---

_Merged by @MichaReiser on 2025-01-08 14:56_

---

_Closed by @MichaReiser on 2025-01-08 14:56_

---

_Branch deleted on 2025-01-08 14:56_

---
