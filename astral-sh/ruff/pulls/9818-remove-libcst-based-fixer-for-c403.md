```yaml
number: 9818
title: "Remove LibCST-based fixer for `C403`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/lcst
created_at: 2024-02-05T00:31:18Z
updated_at: 2024-02-05T01:08:20Z
url: https://github.com/astral-sh/ruff/pull/9818
synced_at: 2026-01-12T15:55:30Z
```

# Remove LibCST-based fixer for `C403`

---

_@charliermarsh_

## Summary

Experimenting with rewriting one of the comprehension fixes _without_ LibCST.

---

_Comment by @charliermarsh on 2024-02-05 00:43_

This actually does a _better_ job of preserving comments.

---

_Marked ready for review by @charliermarsh on 2024-02-05 00:43_

---

_Label `fixes` added by @charliermarsh on 2024-02-05 00:43_

---

_Comment by @github-actions[bot] on 2024-02-05 00:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Remove one usage of LibCST" to "Remove LibCST-based fixer for `C403`" by @charliermarsh on 2024-02-05 01:08_

---

_Merged by @charliermarsh on 2024-02-05 01:08_

---

_Closed by @charliermarsh on 2024-02-05 01:08_

---

_Branch deleted on 2024-02-05 01:08_

---
