```yaml
number: 18664
title: "[syntax-errors] Raise unsupported syntax error for template strings prior to Python 3.14"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - preview
  - python314
assignees: []
merged: true
base: main
head: tstring-unsupported
created_at: 2025-06-13T17:04:13Z
updated_at: 2025-06-13T19:04:37Z
url: https://github.com/astral-sh/ruff/pull/18664
synced_at: 2026-01-12T15:56:23Z
```

# [syntax-errors] Raise unsupported syntax error for template strings prior to Python 3.14

---

_@dylwil3_

Closes #18662

One question is whether we would like the range to exclude the quotes?


---

_Review requested from @MichaReiser by @dylwil3 on 2025-06-13 17:04_

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-06-13 17:04_

---

_Label `bug` added by @dylwil3 on 2025-06-13 17:04_

---

_Label `preview` added by @dylwil3 on 2025-06-13 17:04_

---

_Label `python314` added by @dylwil3 on 2025-06-13 17:04_

---

_Comment by @github-actions[bot] on 2025-06-13 17:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2025-06-13 17:41_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@template_strings_py313.py.snap`:196 on 2025-06-13 17:41_

I think I'd prefer if it highlights the entire string rather than just the prefix. Can you double check how we do it for f-string and implement it the same?

---

_@dylwil3 reviewed on 2025-06-13 18:12_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@template_strings_py313.py.snap`:196 on 2025-06-13 18:12_

We don't have an error for f-strings - they have been around since Python 3.6 😄 
But I'm happy to highlight the whole string - it might be more helpful for implicitly concatenated examples as well

---

_Review requested from @MichaReiser by @dylwil3 on 2025-06-13 18:13_

---

_@MichaReiser approved on 2025-06-13 18:43_

That was quick. Thank you

---

_Merged by @dylwil3 on 2025-06-13 19:04_

---

_Closed by @dylwil3 on 2025-06-13 19:04_

---
