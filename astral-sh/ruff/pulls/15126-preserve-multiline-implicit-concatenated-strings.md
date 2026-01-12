```yaml
number: 15126
title: Preserve multiline implicit concatenated strings in docstring positions
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/preserve-multiline-implicit-concatenated-string-in-docstrings
created_at: 2024-12-23T15:41:19Z
updated_at: 2025-01-03T09:27:15Z
url: https://github.com/astral-sh/ruff/pull/15126
synced_at: 2026-01-12T15:55:50Z
```

# Preserve multiline implicit concatenated strings in docstring positions

---

_@MichaReiser_

## Summary
This is a bug fix for https://github.com/astral-sh/ruff/pull/13928

I noticed this when writing up the black deviations document and was very confused until I realized... Oh, it's a docstring!

We failed to preserve multiline implicit concatenated
strings that can't be automatically joined in docstring positions:

```py
def test():
  (
     r"This is a docstring"
     "and Ruff can't automatically join it"
  )
```

## Test plan
Added formatter tests


---

_Label `formatter` added by @MichaReiser on 2024-12-23 15:41_

---

_Label `preview` added by @MichaReiser on 2024-12-23 15:41_

---

_@MichaReiser reviewed on 2024-12-23 15:41_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_string_literal.rs`:36 on 2024-12-23 15:41_

Clippy forced me to invert the if and else branches.

---

_Marked ready for review by @MichaReiser on 2024-12-23 15:41_

---

_Comment by @github-actions[bot] on 2024-12-23 15:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila approved on 2024-12-24 05:38_

---

_Merged by @MichaReiser on 2025-01-03 09:27_

---

_Closed by @MichaReiser on 2025-01-03 09:27_

---

_Branch deleted on 2025-01-03 09:27_

---
