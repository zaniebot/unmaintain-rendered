```yaml
number: 15340
title: "[`flake8-pyi`] Stabilize: include all python file types for `PYI006`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: ruff-0.9
head: micha/sys-version-comparison
created_at: 2025-01-08T09:11:14Z
updated_at: 2025-01-08T13:08:36Z
url: https://github.com/astral-sh/ruff/pull/15340
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-pyi`] Stabilize: include all python file types for `PYI006`

---

_@MichaReiser_

## Summary

Stabilizes the behavior changes introduced in https://github.com/astral-sh/ruff/pull/14059

`PYI006` ~~and `PYI066`~~ now also apply to non stub files. 

## Test plan

There are no open or recently closed issues around `PYI006` ~~or `PYI066`~~




---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-08 09:11_

---

_Label `rule` added by @MichaReiser on 2025-01-08 09:15_

---

_@MichaReiser reviewed on 2025-01-08 09:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1202 on 2025-01-08 09:16_

The diff here looks misleading. All I did is to remove the outermost `if`

---

_Comment by @github-actions[bot] on 2025-01-08 09:17_

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
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/decorators/condition.py#L32'>airflow/decorators/condition.py:32:35:</a> TC008 [*] Remove quotes from type alias
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/decorators/condition.py#L33'>airflow/decorators/condition.py:33:35:</a> TC008 [*] Remove quotes from type alias
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

_Comment by @MichaReiser on 2025-01-08 09:22_

I'd like to have a second opinion on a few ecosystem results. There are instances where the `sys.version` check is part of an `and` or `or` condition. Should the rule still apply in those cases? 

* https://github.com/pytest-dev/pytest/blob/5d58b1fa7822fb1b8ff28cacc8a3c37e521e5cc3/src/_pytest/_code/code.py#L707
* https://github.com/scikit-build/scikit-build-core/blob/241c63bcec484b63f1c9d95eb696414cfc02f6c8/tests/test_builder.py#L247

Rewriting some of those just feels slightly more awkward


---

_Added to milestone `v0.9` by @MichaReiser on 2025-01-08 10:29_

---

_Comment by @AlexWaygood on 2025-01-08 10:59_

> I'd like to have a second opinion on a few ecosystem results. There are instances where the `sys.version` check is part of an `and` or `or` condition. Should the rule still apply in those cases?

That's a great catch. No, I don't think it should apply in those cases. Let's hold off on stabilising the PYI066 change, in that case.

---

_Comment by @MichaReiser on 2025-01-08 12:23_

I created an issue to follow up on `PYI066` (https://github.com/astral-sh/ruff/issues/15347) and are about to roll back the `PYI066` changes

---

_Renamed from "[`flake8-pyi`] Stabilize: include all python file types for `PYI006` and `PYI066`" to "[`flake8-pyi`] Stabilize: include all python file types for `PYI006`" by @MichaReiser on 2025-01-08 12:24_

---

_@AlexWaygood approved on 2025-01-08 13:06_

Thank you!

---

_Merged by @MichaReiser on 2025-01-08 13:08_

---

_Closed by @MichaReiser on 2025-01-08 13:08_

---

_Branch deleted on 2025-01-08 13:08_

---
