```yaml
number: 12771
title: "Don't enforce returns and yields in abstract methods"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/abs
created_at: 2024-08-09T02:19:34Z
updated_at: 2024-08-09T13:43:33Z
url: https://github.com/astral-sh/ruff/pull/12771
synced_at: 2026-01-12T15:55:42Z
```

# Don't enforce returns and yields in abstract methods

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12685.


---

_Review requested from @AlexWaygood by @charliermarsh on 2024-08-09 02:28_

---

_Label `bug` added by @charliermarsh on 2024-08-09 02:28_

---

_Label `docstring` added by @charliermarsh on 2024-08-09 02:28_

---

_@charliermarsh reviewed on 2024-08-09 02:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:720 on 2024-08-09 02:28_

I guess we could enforce _some_ of the rules, e.g., if there's a default implementation that has a raise, and it's not documented? So, only omit the "extraneous" rules?

---

_Comment by @github-actions[bot] on 2024-08-09 02:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-09 06:08_

---

_@AlexWaygood reviewed on 2024-08-09 09:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:720 on 2024-08-09 09:16_

Yeah, I think I'd continue to enforce the rules that say "you returned/raised/yielded something but didn't document it", and I'd skip the ones that say "you documented that it returns/raises/yields something it doesn't". Abstract methods can have implementations that are called using `super()` from concrete methods in subclasses

---

_@charliermarsh reviewed on 2024-08-09 13:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:720 on 2024-08-09 13:28_

Kk

---

_Merged by @charliermarsh on 2024-08-09 13:34_

---

_Closed by @charliermarsh on 2024-08-09 13:34_

---

_Branch deleted on 2024-08-09 13:34_

---

_@AlexWaygood reviewed on 2024-08-09 13:34_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:28 on 2024-08-09 13:34_

```suggestion
/// This rule is not enforced for stub functions.
```

---
