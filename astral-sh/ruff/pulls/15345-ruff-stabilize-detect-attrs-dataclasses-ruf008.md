```yaml
number: 15345
title: "[`ruff`] Stabilize: Detect `attrs` dataclasses (`RUF008`, `RUF009`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: ruff-0.9
head: micha/attrs-dataclasses
created_at: 2025-01-08T10:28:41Z
updated_at: 2025-01-08T11:57:23Z
url: https://github.com/astral-sh/ruff/pull/15345
synced_at: 2026-01-12T15:55:51Z
```

# [`ruff`] Stabilize: Detect `attrs` dataclasses (`RUF008`, `RUF009`)

---

_@MichaReiser_

## Summary

This PR stabilizes the rule changes introduced in https://github.com/astral-sh/ruff/pull/14327



## Test Plan

There are a few open issues for `RUF009` and one that affects both `RUF008` and `RUF009`:

* https://github.com/astral-sh/ruff/issues/4171: This issue requests a rule extension. It shouldn't prevent stabilizing this behavior
* https://github.com/astral-sh/ruff/issues/6447: Is a false-positive. It also applies to the extension introduced in #14327 (the behavior this PR stabilizes) but is not specific. That's why I think it's okay to go ahead with stabilizing the behavior change.
* https://github.com/astral-sh/ruff/issues/12288: This is specific to stringified type annotations. My reasoning is the same as for the above issue. The false-positive isn't specific to the behavior change that this PR stabilizes and shouldn't block it.

The ecosystem changes look good to me

---

_Renamed from "[`ruff`] Stabilize: Detect `attrs` dataclasses (RUF008, RUF009)" to "[`ruff`] Stabilize: Detect `attrs` dataclasses (`RUF008`, `RUF009`)" by @MichaReiser on 2025-01-08 10:28_

---

_Label `rule` added by @MichaReiser on 2025-01-08 10:28_

---

_Added to milestone `v0.9` by @MichaReiser on 2025-01-08 10:29_

---

_Comment by @github-actions[bot] on 2025-01-08 10:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+12 -6 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L64'>airflow/lineage/entities.py:64:23:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L64'>airflow/lineage/entities.py:64:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L86'>airflow/lineage/entities.py:86:23:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L86'>airflow/lineage/entities.py:86:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L88'>airflow/lineage/entities.py:88:29:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L88'>airflow/lineage/entities.py:88:29:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L89'>airflow/lineage/entities.py:89:26:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L89'>airflow/lineage/entities.py:89:26:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L90'>airflow/lineage/entities.py:90:29:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/lineage/entities.py#L90'>airflow/lineage/entities.py:90:29:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/models/dag.py#L430'>airflow/models/dag.py:430:25:</a> RUF009 Do not perform function call in dataclass defaults
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/models/dag.py#L431'>airflow/models/dag.py:431:24:</a> RUF009 Do not perform function call `airflow_conf.get_mandatory_value` in dataclass defaults
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/papermill/operators/papermill.py#L42'>providers/src/airflow/providers/papermill/operators/papermill.py:42:31:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/papermill/operators/papermill.py#L42'>providers/src/airflow/providers/papermill/operators/papermill.py:42:31:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/tests/units/test_attribute_access_type.py#L303'>tests/units/test_attribute_access_type.py:303:27:</a> RUF008 Do not use mutable default values for dataclass attributes
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/tests/units/test_attribute_access_type.py#L304'>tests/units/test_attribute_access_type.py:304:27:</a> RUF008 Do not use mutable default values for dataclass attributes
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/tests/units/test_attribute_access_type.py#L307'>tests/units/test_attribute_access_type.py:307:31:</a> RUF008 Do not use mutable default values for dataclass attributes
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/tests/units/test_attribute_access_type.py#L308'>tests/units/test_attribute_access_type.py:308:36:</a> RUF008 Do not use mutable default values for dataclass attributes
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF008 | 10 | 10 | 0 | 0 | 0 |
| RUF012 | 6 | 0 | 6 | 0 | 0 |
| RUF009 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-01-08 11:11_

---

_Merged by @MichaReiser on 2025-01-08 11:57_

---

_Closed by @MichaReiser on 2025-01-08 11:57_

---

_Branch deleted on 2025-01-08 11:57_

---
