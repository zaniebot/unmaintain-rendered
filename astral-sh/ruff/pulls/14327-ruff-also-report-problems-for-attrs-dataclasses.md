```yaml
number: 14327
title: "[`ruff`] Also report problems for `attrs` dataclasses in preview mode (`RUF008`, `RUF009`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF008
created_at: 2024-11-14T00:06:26Z
updated_at: 2025-03-04T10:26:16Z
url: https://github.com/astral-sh/ruff/pull/14327
synced_at: 2026-01-10T19:49:01Z
```

# [`ruff`] Also report problems for `attrs` dataclasses in preview mode (`RUF008`, `RUF009`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-14 00:06_

## Summary

Resolves #9757.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-14 00:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -6 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L64'>airflow/lineage/entities.py:64:23:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L64'>airflow/lineage/entities.py:64:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L86'>airflow/lineage/entities.py:86:23:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L86'>airflow/lineage/entities.py:86:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L88'>airflow/lineage/entities.py:88:29:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L88'>airflow/lineage/entities.py:88:29:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L89'>airflow/lineage/entities.py:89:26:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L89'>airflow/lineage/entities.py:89:26:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L90'>airflow/lineage/entities.py:90:29:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/lineage/entities.py#L90'>airflow/lineage/entities.py:90:29:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/models/dag.py#L433'>airflow/models/dag.py:433:25:</a> RUF009 Do not perform function call in dataclass defaults
+ <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/airflow/models/dag.py#L434'>airflow/models/dag.py:434:24:</a> RUF009 Do not perform function call `airflow_conf.get_mandatory_value` in dataclass defaults
+ <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/providers/src/airflow/providers/papermill/operators/papermill.py#L41'>providers/src/airflow/providers/papermill/operators/papermill.py:41:31:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/61076f0ab57feda67ab4d013ed41365774be9bbd/providers/src/airflow/providers/papermill/operators/papermill.py#L41'>providers/src/airflow/providers/papermill/operators/papermill.py:41:31:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF008 | 6 | 6 | 0 | 0 | 0 |
| RUF012 | 6 | 0 | 6 | 0 | 0 |
| RUF009 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @codspeed-hq[bot] on 2024-11-14 01:05_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo:RUF008)

### Merging #14327 will **not alter performance**

<sub>Comparing <code>InSyncWithFoo:RUF008</code> (a958a82) with <code>main</code> (9a3001b)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Label `rule` added by @MichaReiser on 2024-11-14 08:10_

---

_Comment by @MichaReiser on 2024-11-14 08:11_

@AlexWaygood would you mind taking a look at this rule. I don't have the necessary context about what it changes to review it. 

I do think that we should make this a preview-only change because it increases the rule's scope.

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-14 08:11_

---

_@AlexWaygood approved on 2024-11-14 15:07_

Thanks @InSyncWithFoo, this is great! I pushed some changes:
- Optimised it a bit by adding some tracking to the semantic model of whether `attrs` has been imported or not
- Consolidated the logic in the `rules::ruff::helpers` module
- Made the change preview-only for now, since it increases the scope of the rule.

---

_Label `preview` added by @AlexWaygood on 2024-11-14 15:11_

---

_Renamed from "[`ruff`] Also report problems for `attrs` dataclasses (`RUF008`, `RUF009`)" to "[`ruff`] Also report problems for `attrs` dataclasses in preview mode (`RUF008`, `RUF009`)" by @AlexWaygood on 2024-11-14 15:12_

---

_Merged by @AlexWaygood on 2024-11-14 15:13_

---

_Closed by @AlexWaygood on 2024-11-14 15:13_

---

_Branch deleted on 2024-11-14 18:46_

---

_Comment by @pwuertz on 2024-11-16 00:35_

Note that `RUF009` can be a bit misleading when `auto_attribs=False` (or with older `attr.s` codebases):
```python
import attrs

@attrs.define(auto_attribs=False)
class X:
    a_field: int = attrs.field(default=42)
    not_a_field: list = (lambda: [])()
#                       ^^^^^^^^^^^^^^ RUF009 Do not perform function call in dataclass defaults
```
Without `auto_attribs`, `not_a_field` can't possibly be a  `dataclass` field. In this example, it's an incorrectly typed class variable. Fixing the typing error also fixes the `RUF009` warning:
```python
@attrs.define(auto_attribs=False)
class X:
    a_field: int = attrs.field(default=42)
    not_a_field: typing.ClassVar[list] = (lambda: [])()
```

---

_Comment by @pwuertz on 2024-11-16 00:53_

This is an issue though when using `attrs.field` wrappers:

```python
def my_field():
    return attrs.field(default=42, metadata={"my_metadata": 42})


@attrs.define(auto_attribs=False)
class X:
    a_field: int = my_field()
#                  ^^^^^^^^^^ RUF009 Do not perform function call `my_field` in dataclass defaults
```

---

_Comment by @InSyncWithFoo on 2024-11-16 02:19_

@pwuertz I'll work on a fix. Could you open a new issue with all the details, please?

---

_Comment by @ashb on 2025-03-04 10:22_

I can't get this working, even on 0.9.0 which was when this was merged:

```
❯ cat tst.py
import attrs

@attrs.deinfe
class B :
    mutable_default: list[int] = []
    mutable_default2: list[int] = attrs.field(default=[])

❯ uvx ruff@0.9.0 check --extend-select RUF008,B006 --preview -n --isolated  tst.py
All checks passed!

---

_Comment by @MichaReiser on 2025-03-04 10:26_

> I can't get this working, even on 0.9.0 which was when this was merged:
> 
> ```
> ❯ cat tst.py
> import attrs
> 
> @attrs.deinfe
> class B :
>     mutable_default: list[int] = []
>     mutable_default2: list[int] = attrs.field(default=[])
> 
> ❯ uvx ruff@0.9.0 check --extend-select RUF008,B006 --preview -n --isolated  tst.py
> All checks passed!
> ```

Please open a new issue. It makes it easier to help and allows other users to find it too

---
