```yaml
number: 10894
title: "[`pylint`] Recode `nan-comparison` rule to `W0177`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/nan
created_at: 2024-04-12T02:16:00Z
updated_at: 2024-04-12T02:49:21Z
url: https://github.com/astral-sh/ruff/pull/10894
synced_at: 2026-01-12T15:55:33Z
```

# [`pylint`] Recode `nan-comparison` rule to `W0177`

---

_@charliermarsh_

## Summary

This was accidentally committed under `W0117`, but the actual Pylint code is `W0177`: https://pylint.readthedocs.io/en/latest/user_guide/checkers/features.html.

Closes https://github.com/astral-sh/ruff/issues/10791.


---

_Label `bug` added by @charliermarsh on 2024-04-12 02:16_

---

_Comment by @github-actions[bot] on 2024-04-12 02:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+50 -50 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/particles/ionization_state.py#L705'>plasmapy/particles/ionization_state.py:705:12:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/particles/ionization_state.py#L705'>plasmapy/particles/ionization_state.py:705:12:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/particles/particle_class.py#L1944'>plasmapy/particles/particle_class.py:1944:34:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/particles/particle_class.py#L1944'>plasmapy/particles/particle_class.py:1944:34:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+48 -48 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/arrow/array.py#L1403'>pandas/core/arrays/arrow/array.py:1403:29:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/arrow/array.py#L1403'>pandas/core/arrays/arrow/array.py:1403:29:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/categorical.py#L1563'>pandas/core/arrays/categorical.py:1563:82:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/categorical.py#L1563'>pandas/core/arrays/categorical.py:1563:82:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/string_arrow.py#L710'>pandas/core/arrays/string_arrow.py:710:20:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/string_arrow.py#L710'>pandas/core/arrays/string_arrow.py:710:20:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/base.py#L639'>pandas/core/base.py:639:34:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/base.py#L639'>pandas/core/base.py:639:34:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/internals/managers.py#L1000'>pandas/core/internals/managers.py:1000:48:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/internals/managers.py#L1000'>pandas/core/internals/managers.py:1000:48:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/strings/accessor.py#L347'>pandas/core/strings/accessor.py:347:63:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/strings/accessor.py#L347'>pandas/core/strings/accessor.py:347:63:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/strings/object_array.py#L102'>pandas/core/strings/object_array.py:102:28:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/strings/object_array.py#L102'>pandas/core/strings/object_array.py:102:28:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arithmetic/test_object.py#L71'>pandas/tests/arithmetic/test_object.py:71:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arithmetic/test_object.py#L71'>pandas/tests/arithmetic/test_object.py:71:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arithmetic/test_object.py#L75'>pandas/tests/arithmetic/test_object.py:75:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arithmetic/test_object.py#L75'>pandas/tests/arithmetic/test_object.py:75:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/categorical/test_analytics.py#L107'>pandas/tests/arrays/categorical/test_analytics.py:107:30:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/categorical/test_analytics.py#L107'>pandas/tests/arrays/categorical/test_analytics.py:107:30:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/categorical/test_analytics.py#L117'>pandas/tests/arrays/categorical/test_analytics.py:117:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/categorical/test_analytics.py#L117'>pandas/tests/arrays/categorical/test_analytics.py:117:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/categorical/test_indexing.py#L292'>pandas/tests/arrays/categorical/test_indexing.py:292:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/categorical/test_indexing.py#L292'>pandas/tests/arrays/categorical/test_indexing.py:292:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/categorical/test_indexing.py#L301'>pandas/tests/arrays/categorical/test_indexing.py:301:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/categorical/test_indexing.py#L301'>pandas/tests/arrays/categorical/test_indexing.py:301:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/floating/test_contains.py#L12'>pandas/tests/arrays/floating/test_contains.py:12:12:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/arrays/floating/test_contains.py#L12'>pandas/tests/arrays/floating/test_contains.py:12:12:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/frame/methods/test_astype.py#L762'>pandas/tests/frame/methods/test_astype.py:762:28:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/frame/methods/test_astype.py#L762'>pandas/tests/frame/methods/test_astype.py:762:28:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/frame/methods/test_replace.py#L1494'>pandas/tests/frame/methods/test_replace.py:1494:21:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/frame/methods/test_replace.py#L1494'>pandas/tests/frame/methods/test_replace.py:1494:21:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/groupby/test_categorical.py#L1496'>pandas/tests/groupby/test_categorical.py:1496:20:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/groupby/test_categorical.py#L1496'>pandas/tests/groupby/test_categorical.py:1496:20:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L345'>pandas/tests/indexes/categorical/test_indexing.py:345:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L345'>pandas/tests/indexes/categorical/test_indexing.py:345:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L353'>pandas/tests/indexes/categorical/test_indexing.py:353:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L353'>pandas/tests/indexes/categorical/test_indexing.py:353:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L366'>pandas/tests/indexes/categorical/test_indexing.py:366:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L366'>pandas/tests/indexes/categorical/test_indexing.py:366:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L376'>pandas/tests/indexes/categorical/test_indexing.py:376:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L376'>pandas/tests/indexes/categorical/test_indexing.py:376:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L386'>pandas/tests/indexes/categorical/test_indexing.py:386:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/categorical/test_indexing.py#L386'>pandas/tests/indexes/categorical/test_indexing.py:386:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/multi/test_indexing.py#L822'>pandas/tests/indexes/multi/test_indexing.py:822:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/multi/test_indexing.py#L822'>pandas/tests/indexes/multi/test_indexing.py:822:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
- <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/multi/test_indexing.py#L825'>pandas/tests/indexes/multi/test_indexing.py:825:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/tests/indexes/multi/test_indexing.py#L825'>pandas/tests/indexes/multi/test_indexing.py:825:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
... 48 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0177 | 50 | 50 | 0 | 0 | 0 |
| PLW0117 | 50 | 0 | 50 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-04-12 02:49_

---

_Closed by @charliermarsh on 2024-04-12 02:49_

---

_Branch deleted on 2024-04-12 02:49_

---
