```yaml
number: 9219
title: Make parent non-Optional in traverse_union
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/traverse
created_at: 2023-12-20T20:17:56Z
updated_at: 2023-12-21T21:16:38Z
url: https://github.com/astral-sh/ruff/pull/9219
synced_at: 2026-01-12T15:55:28Z
```

# Make parent non-Optional in traverse_union

---

_@charliermarsh_

## Summary

This protects callers from having to pass in `None`, and allows the callback to operate as if it's always a union member.

---

_Label `internal` added by @charliermarsh on 2023-12-20 20:18_

---

_Comment by @github-actions[bot] on 2023-12-20 20:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/support/plugins/selenium.py#L118'>tests/support/plugins/selenium.py:118:88:</a> RUF020 [*] `NoReturn | T` is equivalent to `T`
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/support/plugins/selenium.py#L132'>tests/support/plugins/selenium.py:132:47:</a> RUF020 [*] `NoReturn | T` is equivalent to `T`
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/support/plugins/selenium.py#L146'>tests/support/plugins/selenium.py:146:47:</a> RUF020 [*] `NoReturn | T` is equivalent to `T`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF020 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-12-21 21:10_

---

_Closed by @charliermarsh on 2023-12-21 21:10_

---

_Branch deleted on 2023-12-21 21:10_

---
