```yaml
number: 9677
title: "Document `literal-membership` fix safety conditions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-01-29T17:24:21Z
updated_at: 2024-01-29T17:55:01Z
url: https://github.com/astral-sh/ruff/pull/9677
synced_at: 2026-01-12T15:55:29Z
```

# Document `literal-membership` fix safety conditions

---

_@charliermarsh_

## Summary

This seems safe to me. See https://github.com/astral-sh/ruff/issues/8482#issuecomment-1859299411.

## Test Plan

`cargo test`


---

_Label `preview` added by @charliermarsh on 2024-01-29 17:24_

---

_Added to milestone `v0.2.0` by @charliermarsh on 2024-01-29 17:24_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-29 17:25_

---

_Comment by @charliermarsh on 2024-01-29 17:25_

For 0.2.

---

_Comment by @zanieb on 2024-01-29 17:30_

Isn't this unsafe because the inner type may not be hashable?

---

_Comment by @charliermarsh on 2024-01-29 17:36_

Good call, thank you! I'll just change this to a docs update and merge.

---

_Comment by @github-actions[bot] on 2024-01-29 17:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -180 violations, +0 -0 fixes in 5 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/utils/tensorflow/test_rasa_layers.py#L832'>tests/utils/tensorflow/test_rasa_layers.py:832:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/alteryx/featuretools/blob/3681bbb6ceceed9f820473e5055c5bed7a3771bb/featuretools/entityset/entityset.py#L402'>featuretools/entityset/entityset.py:402:17:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/crossfilter/main.py#L32'>examples/server/app/crossfilter/main.py:32:35:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/e087826a3232f058c07d16b780de40c4e1fab68c/ibis/backends/bigquery/tests/system/test_client.py#L222'>ibis/backends/bigquery/tests/system/test_client.py:222:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/ibis-project/ibis/blob/e087826a3232f058c07d16b780de40c4e1fab68c/ibis/backends/bigquery/tests/system/test_client.py#L223'>ibis/backends/bigquery/tests/system/test_client.py:223:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -175 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/_numba/extensions.py#L52'>pandas/core/_numba/extensions.py:52:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/algorithms.py#L533'>pandas/core/algorithms.py:533:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/algorithms.py#L790'>pandas/core/algorithms.py:790:36:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/algorithms.py#L940'>pandas/core/algorithms.py:940:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/algorithms.py#L940'>pandas/core/algorithms.py:940:38:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/array_algos/putmask.py#L138'>pandas/core/array_algos/putmask.py:138:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/array_algos/putmask.py#L44'>pandas/core/array_algos/putmask.py:44:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/datetimelike.py#L661'>pandas/core/arrays/datetimelike.py:661:40:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/datetimelike.py#L774'>pandas/core/arrays/datetimelike.py:774:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/datetimelike.py#L780'>pandas/core/arrays/datetimelike.py:780:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/datetimes.py#L2426'>pandas/core/arrays/datetimes.py:2426:10:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/interval.py#L257'>pandas/core/arrays/interval.py:257:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/masked.py#L968'>pandas/core/arrays/masked.py:968:30:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/numeric.py#L164'>pandas/core/arrays/numeric.py:164:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/string_arrow.py#L656'>pandas/core/arrays/string_arrow.py:656:32:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/timedeltas.py#L1040'>pandas/core/arrays/timedeltas.py:1040:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/arrays/timedeltas.py#L647'>pandas/core/arrays/timedeltas.py:647:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/computation/expressions.py#L200'>pandas/core/computation/expressions.py:200:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/construction.py#L551'>pandas/core/construction.py:551:39:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/construction.py#L605'>pandas/core/construction.py:605:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/construction.py#L652'>pandas/core/construction.py:652:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/astype.py#L103'>pandas/core/dtypes/astype.py:103:10:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/astype.py#L131'>pandas/core/dtypes/astype.py:131:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/cast.py#L1036'>pandas/core/dtypes/cast.py:1036:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/cast.py#L1065'>pandas/core/dtypes/cast.py:1065:21:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/cast.py#L1091'>pandas/core/dtypes/cast.py:1091:21:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/cast.py#L1179'>pandas/core/dtypes/cast.py:1179:45:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/cast.py#L1700'>pandas/core/dtypes/cast.py:1700:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/dtypes.py#L450'>pandas/core/dtypes/dtypes.py:450:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/dtypes/missing.py#L643'>pandas/core/dtypes/missing.py:643:10:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/frame.py#L11535'>pandas/core/frame.py:11535:33:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/frame.py#L2482'>pandas/core/frame.py:2482:24:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/frame.py#L8129'>pandas/core/frame.py:8129:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/groupby/generic.py#L1670'>pandas/core/groupby/generic.py:1670:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/groupby/groupby.py#L1977'>pandas/core/groupby/groupby.py:1977:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexers/utils.py#L321'>pandas/core/indexers/utils.py:321:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/base.py#L5558'>pandas/core/indexes/base.py:5558:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/base.py#L7013'>pandas/core/indexes/base.py:7013:56:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/base.py#L7031'>pandas/core/indexes/base.py:7031:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/base.py#L7051'>pandas/core/indexes/base.py:7051:17:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/base.py#L7054'>pandas/core/indexes/base.py:7054:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/base.py#L7054'>pandas/core/indexes/base.py:7054:37:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/base.py#L7124'>pandas/core/indexes/base.py:7124:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/base.py#L873'>pandas/core/indexes/base.py:873:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/indexes/datetimelike.py#L156'>pandas/core/indexes/datetimelike.py:156:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/internals/concat.py#L371'>pandas/core/internals/concat.py:371:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/internals/managers.py#L509'>pandas/core/internals/managers.py:509:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/pandas-dev/pandas/blob/56b5979f136e72ce78e5221392739481c025d5e7/pandas/core/nanops.py#L1682'>pandas/core/nanops.py:1682:14:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
... 127 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E721 | 180 | 0 | 180 | 0 | 0 |

</p>
</details>




---

_Label `preview` removed by @charliermarsh on 2024-01-29 17:42_

---

_Label `documentation` added by @charliermarsh on 2024-01-29 17:42_

---

_Renamed from "Convert `literal-membership` to a safe edit" to "Document `literal-membership` fix safety conditions" by @charliermarsh on 2024-01-29 17:42_

---

_Merged by @charliermarsh on 2024-01-29 17:48_

---

_Closed by @charliermarsh on 2024-01-29 17:48_

---

_Branch deleted on 2024-01-29 17:48_

---
