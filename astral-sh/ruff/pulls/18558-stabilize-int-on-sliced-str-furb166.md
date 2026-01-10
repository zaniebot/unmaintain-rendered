```yaml
number: 18558
title: "Stabilize `int-on-sliced-str` (`FURB166`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-furb166
created_at: 2025-06-08T19:07:28Z
updated_at: 2025-06-09T18:12:12Z
url: https://github.com/astral-sh/ruff/pull/18558
synced_at: 2026-01-10T18:45:04Z
```

# Stabilize `int-on-sliced-str` (`FURB166`)

---

_Pull request opened by @dylwil3 on 2025-06-08 19:07_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:07_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:07_

---

_Comment by @github-actions[bot] on 2025-06-08 19:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/votable/converters.py#L887'>astropy/io/votable/converters.py:887:25:</a> FURB166 Use of `int` with explicit `base=16` after removing prefix
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB166 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:29_

---

_@ntBre approved on 2025-06-09 18:05_

---

_Merged by @dylwil3 on 2025-06-09 18:12_

---

_Closed by @dylwil3 on 2025-06-09 18:12_

---

_Branch deleted on 2025-06-09 18:12_

---
