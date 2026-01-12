```yaml
number: 18559
title: "Stabilize `nan-comparison` (`PLW0177`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-plw0177
created_at: 2025-06-08T19:09:11Z
updated_at: 2025-06-09T19:05:15Z
url: https://github.com/astral-sh/ruff/pull/18559
synced_at: 2026-01-12T15:56:21Z
```

# Stabilize `nan-comparison` (`PLW0177`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:09_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:09_

---

_Comment by @github-actions[bot] on 2025-06-08 19:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+94 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/1f8e45d83b202e6a7e1e0ce26eb67e2c31cd6346/src/plasmapy/particles/ionization_state.py#L701'>src/plasmapy/particles/ionization_state.py:701:12:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/1f8e45d83b202e6a7e1e0ce26eb67e2c31cd6346/src/plasmapy/particles/particle_class.py#L2133'>src/plasmapy/particles/particle_class.py:2133:34:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+91 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/_testing/asserters.py#L813'>pandas/_testing/asserters.py:813:36:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/_testing/asserters.py#L821'>pandas/_testing/asserters.py:821:37:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/arrow/array.py#L1458'>pandas/core/arrays/arrow/array.py:1458:29:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/categorical.py#L1583'>pandas/core/arrays/categorical.py:1583:82:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/categorical.py#L2703'>pandas/core/arrays/categorical.py:2703:46:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_.py#L1071'>pandas/core/arrays/string_.py:1071:39:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_.py#L414'>pandas/core/arrays/string_.py:414:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_.py#L857'>pandas/core/arrays/string_.py:857:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_.py#L954'>pandas/core/arrays/string_.py:954:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_arrow.py#L223'>pandas/core/arrays/string_arrow.py:223:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_arrow.py#L223'>pandas/core/arrays/string_arrow.py:223:54:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_arrow.py#L243'>pandas/core/arrays/string_arrow.py:243:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_arrow.py#L416'>pandas/core/arrays/string_arrow.py:416:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_arrow.py#L428'>pandas/core/arrays/string_arrow.py:428:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_arrow.py#L440'>pandas/core/arrays/string_arrow.py:440:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_arrow.py#L468'>pandas/core/arrays/string_arrow.py:468:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/arrays/string_arrow.py#L485'>pandas/core/arrays/string_arrow.py:485:35:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/base.py#L681'>pandas/core/base.py:681:34:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/dtypes/cast.py#L1109'>pandas/core/dtypes/cast.py:1109:43:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/indexes/base.py#L5486'>pandas/core/indexes/base.py:5486:40:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/internals/managers.py#L994'>pandas/core/internals/managers.py:994:48:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/strings/accessor.py#L366'>pandas/core/strings/accessor.py:366:63:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/core/strings/object_array.py#L109'>pandas/core/strings/object_array.py:109:28:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arithmetic/test_object.py#L69'>pandas/tests/arithmetic/test_object.py:69:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arithmetic/test_object.py#L73'>pandas/tests/arithmetic/test_object.py:73:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/categorical/test_analytics.py#L107'>pandas/tests/arrays/categorical/test_analytics.py:107:30:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/categorical/test_analytics.py#L117'>pandas/tests/arrays/categorical/test_analytics.py:117:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/categorical/test_indexing.py#L292'>pandas/tests/arrays/categorical/test_indexing.py:292:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/categorical/test_indexing.py#L301'>pandas/tests/arrays/categorical/test_indexing.py:301:16:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/floating/test_contains.py#L12'>pandas/tests/arrays/floating/test_contains.py:12:12:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L100'>pandas/tests/arrays/string_/test_string.py:100:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L106'>pandas/tests/arrays/string_/test_string.py:106:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L115'>pandas/tests/arrays/string_/test_string.py:115:59:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L118'>pandas/tests/arrays/string_/test_string.py:118:58:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L293'>pandas/tests/arrays/string_/test_string.py:293:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L312'>pandas/tests/arrays/string_/test_string.py:312:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L339'>pandas/tests/arrays/string_/test_string.py:339:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L366'>pandas/tests/arrays/string_/test_string.py:366:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L366'>pandas/tests/arrays/string_/test_string.py:366:56:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L418'>pandas/tests/arrays/string_/test_string.py:418:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L505'>pandas/tests/arrays/string_/test_string.py:505:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L624'>pandas/tests/arrays/string_/test_string.py:624:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L670'>pandas/tests/arrays/string_/test_string.py:670:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L683'>pandas/tests/arrays/string_/test_string.py:683:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L700'>pandas/tests/arrays/string_/test_string.py:700:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arrays/string_/test_string.py#L713'>pandas/tests/arrays/string_/test_string.py:713:26:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/extension/base/methods.py#L69'>pandas/tests/extension/base/methods.py:69:78:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/extension/test_string.py#L120'>pandas/tests/extension/test_string.py:120:30:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
... 43 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/4f0eb859eecb79d569247e35235b2c3ec2187b0a/astropy/time/core.py#L1200'>astropy/time/core.py:1200:46:</a> PLW0177 Comparing against a NaN value; use `np.isnan` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0177 | 94 | 94 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:29_

---

_@ntBre approved on 2025-06-09 17:22_

---

_Comment by @ntBre on 2025-06-09 17:25_

These `is np.nan` comparisons in the ecosystem do _appear_ to work, but I think it's still reasonable to emit the diagnostic. For example,

```pycon
>>> import numpy as np
>>> np.nan is np.nan
True
>>> np.nan is float("nan")
False
>>> np.isnan(float("nan"))
np.True_
```

`isnan` is still better in case you somehow get a NaN from outside numpy.

---

_Merged by @dylwil3 on 2025-06-09 19:05_

---

_Closed by @dylwil3 on 2025-06-09 19:05_

---

_Branch deleted on 2025-06-09 19:05_

---
