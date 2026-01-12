```yaml
number: 17740
title: Fix example syntax for pydocstyle ignore_var_parameters option
type: pull_request
state: merged
author: brendancooley
labels:
  - documentation
assignees: []
merged: true
base: main
head: bc/fix-ignore-var-parameters
created_at: 2025-04-30T15:41:53Z
updated_at: 2025-04-30T16:21:41Z
url: https://github.com/astral-sh/ruff/pull/17740
synced_at: 2026-01-12T15:56:05Z
```

# Fix example syntax for pydocstyle ignore_var_parameters option

---

_@brendancooley_

## Summary

I noticed an error in the `pyproject.toml` syntax for the pydocstyle option [`ignore-var-parameters`](https://docs.astral.sh/ruff/settings/#lint_pydocstyle_ignore-var-parameters). This PR switches underscores to hyphens in the example to correct the syntax.

## Test Plan

All checks from [contributor guidelines](https://docs.astral.sh/ruff/contributing/#development) have been run and passed. Additionally, I have confirmed that the corrected example syntax correctly implements the option and avoids parsing errors on pyproject.toml.


---

_Label `documentation` added by @MichaReiser on 2025-04-30 16:07_

---

_Comment by @MichaReiser on 2025-04-30 16:07_

Thank you

---

_Comment by @github-actions[bot] on 2025-04-30 16:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2025-04-30 16:19_

---

_Closed by @MichaReiser on 2025-04-30 16:19_

---

_Branch deleted on 2025-04-30 16:21_

---
