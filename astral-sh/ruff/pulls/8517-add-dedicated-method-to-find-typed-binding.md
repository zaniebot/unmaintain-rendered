```yaml
number: 8517
title: Add dedicated method to find typed binding
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/typing-binding
created_at: 2023-11-06T15:54:32Z
updated_at: 2023-11-06T16:25:34Z
url: https://github.com/astral-sh/ruff/pull/8517
synced_at: 2026-01-10T23:40:55Z
```

# Add dedicated method to find typed binding

---

_Pull request opened by @charliermarsh on 2023-11-06 15:54_

## Summary

We have this pattern in a bunch of places, where we find the _only_ binding to a name (and return `None`) if it's bound multiple times. This PR DRYs it up into a method on `SemanticModel`.

---

_Label `internal` added by @charliermarsh on 2023-11-06 15:56_

---

_Comment by @github-actions[bot] on 2023-11-06 16:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/tests/test_ext_autosummary.py#L157'>tests/test_ext_autosummary.py:157:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF403 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-11-06 16:25_

---

_Closed by @charliermarsh on 2023-11-06 16:25_

---

_Branch deleted on 2023-11-06 16:25_

---
