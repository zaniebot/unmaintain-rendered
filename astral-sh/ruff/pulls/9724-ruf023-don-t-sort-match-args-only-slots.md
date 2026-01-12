```yaml
number: 9724
title: "RUF023: Don't sort `__match_args__`, only `__slots__`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
assignees: []
merged: true
base: main
head: match-args-not-sorted
created_at: 2024-01-30T22:25:06Z
updated_at: 2024-01-30T22:45:02Z
url: https://github.com/astral-sh/ruff/pull/9724
synced_at: 2026-01-12T15:55:30Z
```

# RUF023: Don't sort `__match_args__`, only `__slots__`

---

_@AlexWaygood_

Fixes #9723. I'm pretty embarrassed I forgot that order was important here :(

---

_@charliermarsh approved on 2024-01-30 22:27_

It's all good, burden also falls on the reviewers! Thanks for the quick response to the issue.

---

_Comment by @github-actions[bot] on 2024-01-30 22:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/4fb0aad941029b6fbc47342297b47305f5366d1c/ibis/common/tests/test_graph.py#L25'>ibis/common/tests/test_graph.py:25:22:</a> RUF023 [*] `MyNode.__match_args__` is not sorted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF023 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Merged by @AlexWaygood on 2024-01-30 22:44_

---

_Closed by @AlexWaygood on 2024-01-30 22:44_

---

_Branch deleted on 2024-01-30 22:44_

---

_Label `bug` added by @AlexWaygood on 2024-01-30 22:45_

---
