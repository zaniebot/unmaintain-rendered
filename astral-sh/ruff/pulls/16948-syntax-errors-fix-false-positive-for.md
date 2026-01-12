```yaml
number: 16948
title: "[syntax-errors] Fix false positive for parenthesized tuple index"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/star-args
created_at: 2025-03-24T14:00:13Z
updated_at: 2025-03-24T14:34:41Z
url: https://github.com/astral-sh/ruff/pull/16948
synced_at: 2026-01-12T15:55:59Z
```

# [syntax-errors] Fix false positive for parenthesized tuple index

---

_@ntBre_

Summary
--

Fixes #16943 by checking if the tuple is not parenthesized before emitting an error.

Test Plan
--

New inline test based on the initial report

---

_Label `bug` added by @ntBre on 2025-03-24 14:00_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-24 14:00_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-24 14:00_

---

_Comment by @github-actions[bot] on 2025-03-24 14:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mesonbuild/meson-python/blob/726aafc7235ea45da94605e9d4c606da2c9d376e/mesonpy/_editable.py#L253'>mesonpy/_editable.py:253:31:</a> SyntaxError: Cannot use star expression in index on Python 3.8 (syntax was added in Python 3.11)
- <a href='https://github.com/mesonbuild/meson-python/blob/726aafc7235ea45da94605e9d4c606da2c9d376e/mesonpy/_editable.py#L253'>mesonpy/_editable.py:253:48:</a> SyntaxError: Cannot use star expression in index on Python 3.8 (syntax was added in Python 3.11)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SyntaxError: | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila approved on 2025-03-24 14:23_

---

_Merged by @ntBre on 2025-03-24 14:34_

---

_Closed by @ntBre on 2025-03-24 14:34_

---

_Branch deleted on 2025-03-24 14:34_

---
