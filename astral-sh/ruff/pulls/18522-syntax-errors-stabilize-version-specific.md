```yaml
number: 18522
title: "[syntax errors] Stabilize version-specific unsupported syntax errors"
type: pull_request
state: merged
author: dylwil3
labels: []
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-syntax-errors
created_at: 2025-06-06T23:24:24Z
updated_at: 2025-06-07T16:30:46Z
url: https://github.com/astral-sh/ruff/pull/18522
synced_at: 2026-01-10T18:45:04Z
```

# [syntax errors] Stabilize version-specific unsupported syntax errors

---

_Pull request opened by @dylwil3 on 2025-06-06 23:24_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-06 23:28_

---

_Comment by @github-actions[bot] on 2025-06-06 23:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ cookbook/img-to_img-search_CLIP_ChromaDB.ipynb:cell 27:22:49: SyntaxError: Cannot reuse outer quote character in f-strings on Python 3.9 (syntax was added in Python 3.12)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SyntaxError: | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-06-07 15:20_

Nice!

---

_Merged by @dylwil3 on 2025-06-07 16:30_

---

_Closed by @dylwil3 on 2025-06-07 16:30_

---

_Branch deleted on 2025-06-07 16:30_

---
