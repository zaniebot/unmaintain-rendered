```yaml
number: 3533
title: Avoid unused argument violations in .pyi files
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/arg
created_at: 2023-03-15T03:12:46Z
updated_at: 2023-03-15T03:21:27Z
url: https://github.com/astral-sh/ruff/pull/3533
synced_at: 2026-01-12T04:39:45Z
```

# Avoid unused argument violations in .pyi files

---

_Pull request opened by @charliermarsh on 2023-03-15 03:12_

Closes #3532.

---

_Merged by @charliermarsh on 2023-03-15 03:17_

---

_Closed by @charliermarsh on 2023-03-15 03:17_

---

_Branch deleted on 2023-03-15 03:17_

---

_Comment by @github-actions[bot] on 2023-03-15 03:21_

ℹ️ ecosystem check **detected changes**. (+0, -25, 0 error(s))

<details><summary>bokeh (+0, -18)</summary>
<p>

```diff
- src/typings/IPython/display.pyi:3:26: ARG001 Unused function argument: `data`
- src/typings/IPython/display.pyi:3:48: ARG001 Unused function argument: `metadata`
- src/typings/IPython/display.pyi:4:34: ARG001 Unused function argument: `transient`
- src/typings/IPython/display.pyi:4:5: ARG001 Unused function argument: `source`
- src/typings/IPython/display.pyi:4:76: ARG001 Unused function argument: `kwargs`
- src/typings/json5.pyi:10:5: ARG001 Unused function argument: `cls`
- src/typings/json5.pyi:11:5: ARG001 Unused function argument: `indent`
- src/typings/json5.pyi:12:5: ARG001 Unused function argument: `separators`
- src/typings/json5.pyi:13:5: ARG001 Unused function argument: `default`
- src/typings/json5.pyi:14:5: ARG001 Unused function argument: `sort_keys`
- src/typings/json5.pyi:15:5: ARG001 Unused function argument: `quote_keys`
- src/typings/json5.pyi:16:5: ARG001 Unused function argument: `trailing_commas`
- src/typings/json5.pyi:17:5: ARG001 Unused function argument: `allow_duplicate_keys`
- src/typings/json5.pyi:5:5: ARG001 Unused function argument: `obj`
- src/typings/json5.pyi:6:5: ARG001 Unused function argument: `skipkeys`
- src/typings/json5.pyi:7:5: ARG001 Unused function argument: `ensure_ascii`
- src/typings/json5.pyi:8:5: ARG001 Unused function argument: `check_circular`
- src/typings/json5.pyi:9:5: ARG001 Unused function argument: `allow_nan`
```

</p>
</details>
<details><summary>airflow (+0, -7)</summary>
<p>

```diff
- airflow/compat/functools.pyi:27:21: ARG001 Unused function argument: `f`
- airflow/compat/functools.pyi:28:11: ARG001 Unused function argument: `f`
- airflow/utils/context.pyi:112:33: ARG001 Unused function argument: `context`
- airflow/utils/context.pyi:112:51: ARG001 Unused function argument: `task`
- airflow/utils/context.pyi:113:26: ARG001 Unused function argument: `source`
- airflow/utils/context.pyi:113:43: ARG001 Unused function argument: `keys`
- airflow/utils/context.pyi:114:31: ARG001 Unused function argument: `source`
```

</p>
</details>

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---
