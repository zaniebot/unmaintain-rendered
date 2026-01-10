```yaml
number: 9683
title: Error if nursery rules are selected without preview
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: release/0.2.0
head: zb/nursery-err-020
created_at: 2024-01-29T18:45:42Z
updated_at: 2024-01-30T15:27:57Z
url: https://github.com/astral-sh/ruff/pull/9683
synced_at: 2026-01-10T22:57:09Z
```

# Error if nursery rules are selected without preview

---

_Pull request opened by @zanieb on 2024-01-29 18:45_

Extends #9682 to error if the nursery selector is used or nursery rules are selected without preview.

Part of #7992 — we will remove this in 0.3.0 instead so we can provide nice errors in 0.2.0.


---

_Added to milestone `v0.2.0` by @zanieb on 2024-01-29 18:51_

---

_Marked ready for review by @zanieb on 2024-01-29 21:08_

---

_Comment by @github-actions[bot] on 2024-01-29 21:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB131
	- RUF017
	- FURB113
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-01-30 07:35_

---

_Merged by @zanieb on 2024-01-30 15:27_

---

_Closed by @zanieb on 2024-01-30 15:27_

---

_Branch deleted on 2024-01-30 15:27_

---
