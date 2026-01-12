```yaml
number: 18555
title: "[`ruff`] Stabilize `invalid-formatter-suppression-comment` (`RUF028`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-ruf028
created_at: 2025-06-08T19:01:06Z
updated_at: 2025-06-09T17:49:02Z
url: https://github.com/astral-sh/ruff/pull/18555
synced_at: 2026-01-12T15:56:21Z
```

# [`ruff`] Stabilize `invalid-formatter-suppression-comment` (`RUF028`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:01_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:01_

---

_Comment by @github-actions[bot] on 2025-06-08 19:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/9e9633de9da7a9fab03b4bba3a326bf85b412050/testing/test_assertrewrite.py#L2200'>testing/test_assertrewrite.py:2200:1:</a> RUF028 This suppression comment is invalid because it cannot be after a decorator
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF028 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:30_

---

_@ntBre approved on 2025-06-09 16:59_

Nice, I checked the ecosystem result, and it's definitely not turning formatting back on!

---

_Merged by @dylwil3 on 2025-06-09 17:49_

---

_Closed by @dylwil3 on 2025-06-09 17:49_

---

_Branch deleted on 2025-06-09 17:49_

---
