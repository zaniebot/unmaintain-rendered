```yaml
number: 10498
title: "Add missing `Options` references to blank line docs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/opt
created_at: 2024-03-21T00:41:07Z
updated_at: 2024-03-21T00:54:17Z
url: https://github.com/astral-sh/ruff/pull/10498
synced_at: 2026-01-10T22:47:02Z
```

# Add missing `Options` references to blank line docs

---

_Pull request opened by @charliermarsh on 2024-03-21 00:41_

See: https://github.com/astral-sh/ruff/issues/10427.

---

_Renamed from "Add missing Options references to blank line docs" to "Add missing `Options` references to blank line docs" by @charliermarsh on 2024-03-21 00:41_

---

_Label `documentation` added by @charliermarsh on 2024-03-21 00:41_

---

_Merged by @charliermarsh on 2024-03-21 00:49_

---

_Closed by @charliermarsh on 2024-03-21 00:49_

---

_Branch deleted on 2024-03-21 00:49_

---

_Comment by @github-actions[bot] on 2024-03-21 00:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+48 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+48 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/core/arrays/arrow/array.py#L1409'>pandas/core/arrays/arrow/array.py:1409:29:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/core/arrays/categorical.py#L1597'>pandas/core/arrays/categorical.py:1597:82:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/core/arrays/string_arrow.py#L710'>pandas/core/arrays/string_arrow.py:710:20:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/core/base.py#L641'>pandas/core/base.py:641:34:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/core/internals/managers.py#L1000'>pandas/core/internals/managers.py:1000:48:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/core/strings/accessor.py#L347'>pandas/core/strings/accessor.py:347:63:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/core/strings/object_array.py#L102'>pandas/core/strings/object_array.py:102:28:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/arithmetic/test_object.py#L71'>pandas/tests/arithmetic/test_object.py:71:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/arithmetic/test_object.py#L75'>pandas/tests/arithmetic/test_object.py:75:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/arrays/categorical/test_analytics.py#L107'>pandas/tests/arrays/categorical/test_analytics.py:107:30:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/arrays/categorical/test_analytics.py#L117'>pandas/tests/arrays/categorical/test_analytics.py:117:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/arrays/categorical/test_indexing.py#L292'>pandas/tests/arrays/categorical/test_indexing.py:292:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/arrays/categorical/test_indexing.py#L301'>pandas/tests/arrays/categorical/test_indexing.py:301:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/arrays/floating/test_contains.py#L12'>pandas/tests/arrays/floating/test_contains.py:12:12:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/frame/methods/test_astype.py#L762'>pandas/tests/frame/methods/test_astype.py:762:28:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/frame/methods/test_replace.py#L1541'>pandas/tests/frame/methods/test_replace.py:1541:21:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/groupby/test_categorical.py#L1496'>pandas/tests/groupby/test_categorical.py:1496:20:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/categorical/test_indexing.py#L345'>pandas/tests/indexes/categorical/test_indexing.py:345:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/categorical/test_indexing.py#L353'>pandas/tests/indexes/categorical/test_indexing.py:353:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/categorical/test_indexing.py#L366'>pandas/tests/indexes/categorical/test_indexing.py:366:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/categorical/test_indexing.py#L376'>pandas/tests/indexes/categorical/test_indexing.py:376:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/categorical/test_indexing.py#L386'>pandas/tests/indexes/categorical/test_indexing.py:386:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/multi/test_indexing.py#L822'>pandas/tests/indexes/multi/test_indexing.py:822:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/multi/test_indexing.py#L825'>pandas/tests/indexes/multi/test_indexing.py:825:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/numeric/test_indexing.py#L533'>pandas/tests/indexes/numeric/test_indexing.py:533:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/period/test_indexing.py#L784'>pandas/tests/indexes/period/test_indexing.py:784:16:</a> PLW0117 Comparing against a NaN value; use `math.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/period/test_indexing.py#L785'>pandas/tests/indexes/period/test_indexing.py:785:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/period/test_indexing.py#L790'>pandas/tests/indexes/period/test_indexing.py:790:16:</a> PLW0117 Comparing against a NaN value; use `math.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/indexes/period/test_indexing.py#L791'>pandas/tests/indexes/period/test_indexing.py:791:16:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/io/excel/test_writers.py#L1021'>pandas/tests/io/excel/test_writers.py:1021:38:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/io/excel/test_writers.py#L1072'>pandas/tests/io/excel/test_writers.py:1072:50:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L150'>pandas/tests/libs/test_libalgos.py:150:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L151'>pandas/tests/libs/test_libalgos.py:151:27:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L152'>pandas/tests/libs/test_libalgos.py:152:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L153'>pandas/tests/libs/test_libalgos.py:153:27:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L154'>pandas/tests/libs/test_libalgos.py:154:27:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L155'>pandas/tests/libs/test_libalgos.py:155:23:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L157'>pandas/tests/libs/test_libalgos.py:157:29:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L158'>pandas/tests/libs/test_libalgos.py:158:30:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L159'>pandas/tests/libs/test_libalgos.py:159:29:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L160'>pandas/tests/libs/test_libalgos.py:160:30:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L161'>pandas/tests/libs/test_libalgos.py:161:30:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/libs/test_libalgos.py#L162'>pandas/tests/libs/test_libalgos.py:162:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/reductions/test_reductions.py#L1362'>pandas/tests/reductions/test_reductions.py:1362:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/reductions/test_reductions.py#L1375'>pandas/tests/reductions/test_reductions.py:1375:30:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/series/indexing/test_setitem.py#L1106'>pandas/tests/series/indexing/test_setitem.py:1106:52:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/series/indexing/test_setitem.py#L619'>pandas/tests/series/indexing/test_setitem.py:619:23:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/d17723c6d447d37c0cb753a517df74705806f4a2/pandas/tests/series/test_constructors.py#L1095'>pandas/tests/series/test_constructors.py:1095:26:</a> PLW0117 Comparing against a NaN value; use `np.isnan` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0117 | 48 | 48 | 0 | 0 | 0 |

</p>
</details>




---
