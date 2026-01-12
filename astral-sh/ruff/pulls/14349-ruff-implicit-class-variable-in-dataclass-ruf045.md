```yaml
number: 14349
title: "[`ruff`] Implicit class variable in dataclass (`RUF045`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF045
created_at: 2024-11-15T00:14:15Z
updated_at: 2025-02-16T07:06:03Z
url: https://github.com/astral-sh/ruff/pull/14349
synced_at: 2026-01-12T15:55:47Z
```

# [`ruff`] Implicit class variable in dataclass (`RUF045`)

---

_@InSyncWithFoo_

## Summary

Resolves #12877.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-15 00:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5e8be07b7a0eed5defced1f548ccb9c0d8b807fa/airflow/executors/base_executor.py#L89'>airflow/executors/base_executor.py:89:5:</a> RUF045 Assignment without annotation found in dataclass body
+ <a href='https://github.com/apache/airflow/blob/5e8be07b7a0eed5defced1f548ccb9c0d8b807fa/dev/breeze/src/airflow_breeze/params/doc_build_params.py#L36'>dev/breeze/src/airflow_breeze/params/doc_build_params.py:36:5:</a> RUF045 Assignment without annotation found in dataclass body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/constants.py#L31'>src/latch_cli/constants.py:31:5:</a> RUF045 Assignment without annotation found in dataclass body
+ <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/constants.py#L32'>src/latch_cli/constants.py:32:5:</a> RUF045 Assignment without annotation found in dataclass body
+ <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/constants.py#L39'>src/latch_cli/constants.py:39:5:</a> RUF045 Assignment without annotation found in dataclass body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/cc8ca939c0477a49fcce0554fa1743bd5c656a11/stubs/fpdf2/fpdf/fonts.pyi#L33'>stubs/fpdf2/fpdf/fonts.pyi:33:5:</a> RUF045 Assignment without annotation found in dataclass body
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/7568be0570ff095ffc63edc55413bc9c67fde4eb/src/scikit_build_core/file_api/model/codemodel.py#L129'>src/scikit_build_core/file_api/model/codemodel.py:129:5:</a> RUF045 Assignment without annotation found in dataclass body
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/7568be0570ff095ffc63edc55413bc9c67fde4eb/src/scikit_build_core/file_api/model/directory.py#L23'>src/scikit_build_core/file_api/model/directory.py:23:5:</a> RUF045 Assignment without annotation found in dataclass body
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF045 | 8 | 8 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2024-11-15 00:51_

False positives found by ecosystem checks:

[`stubs/fpdf2/fpdf/fonts.pyi` (@python/typeshed)](https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stubs/fpdf2/fpdf/fonts.pyi#L33):

```python
@dataclass
class FontFace:
    family: str | None
    emphasis: TextEmphasis | None
    size_pt: int | None
    color: DeviceGray | DeviceRGB | None
    fill_color: DeviceGray | DeviceRGB | None

    ...

    replace = dataclasses.replace
```

[`src/_pytest/fixtures.py` (@pytest-dev/pytest)](https://github.com/pytest-dev/pytest/blob/755083c36cf07ee75952515e2b4abd65e2e91a8f/src/_pytest/fixtures.py#L310):

```python
@dataclasses.dataclass(frozen=True)
class FuncFixtureInfo:
    ...

    __slots__ = ("argnames", "initialnames", "names_closure", "name2fixturedefs")
```

Special names can be exempted, but I'm not sure what to do about the first.

---

_Label `rule` added by @dhruvmanila on 2024-11-18 06:27_

---

_Label `preview` added by @dhruvmanila on 2024-11-18 06:27_

---

_Comment by @MichaReiser on 2024-11-18 08:10_

I'm not saying we shouldn't do it, but requiring type annotations has been controversial in other instances; for example, there's a [long thread for RUF012](https://github.com/astral-sh/ruff/issues/5243) that does something similar. @AlexWaygood I'd be interested on your take on this before I start reviewing the rule itself. 

---

_Label `needs-decision` added by @MichaReiser on 2024-11-25 09:43_

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-11-27 09:23_

---

_Review requested from @dylwil3 by @MichaReiser on 2025-02-14 07:40_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-14 13:33_

---

_Label `needs-decision` removed by @dylwil3 on 2025-02-15 14:56_

---

_@dylwil3 approved on 2025-02-15 15:06_

Thank you! 

Looking at the ecosystem results, it seems fairly common for users to define a private class variable in their dataclasses. So I've restricted the scope of the rule a little to exempt that.

---

_Merged by @dylwil3 on 2025-02-15 15:08_

---

_Closed by @dylwil3 on 2025-02-15 15:08_

---

_Branch deleted on 2025-02-16 07:06_

---
