```yaml
number: 9957
title: Disable top-level docstring formatting for notebooks
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
  - notebook
assignees: []
merged: true
base: main
head: notebook-module-docstring
created_at: 2024-02-12T17:55:52Z
updated_at: 2024-02-12T18:18:19Z
url: https://github.com/astral-sh/ruff/pull/9957
synced_at: 2026-01-10T22:57:09Z
```

# Disable top-level docstring formatting for notebooks

---

_Pull request opened by @MichaReiser on 2024-02-12 17:55_

## Summary

> one can't access the module docstring for a notebook which makes a notebook not having a module level docstring. @dhruv

This PR handles strings at the top of a cell as non-docstrings. Handling them as docstrings has the undesired result that one might have multiple "docstrings" in a single notebook,
which is probably not what you want.

We don't need to gate this behind preview because it doesn't change any formatted code.

## Test Plan

Added tests


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-02-12 17:56_

---

_Label `formatter` added by @MichaReiser on 2024-02-12 17:56_

---

_Label `notebook` added by @MichaReiser on 2024-02-12 17:56_

---

_Label `bug` added by @MichaReiser on 2024-02-12 17:56_

---

_@dhruvmanila reviewed on 2024-02-12 17:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:749 on 2024-02-12 17:57_

I believe you meant to include "module" somewhere in here?

---

_@MichaReiser reviewed on 2024-02-12 18:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:749 on 2024-02-12 18:00_

And here I'm pretending that jetlag isn't affecting me

---

_@dhruvmanila reviewed on 2024-02-12 18:00_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/notebook_docstring.py`:1 on 2024-02-12 18:00_

Can we add another string after this with a semicolon which, I think, should be removed?

---

_@dhruvmanila approved on 2024-02-12 18:01_

---

_Merged by @MichaReiser on 2024-02-12 18:14_

---

_Closed by @MichaReiser on 2024-02-12 18:14_

---

_Branch deleted on 2024-02-12 18:14_

---

_Comment by @github-actions[bot] on 2024-02-12 18:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
