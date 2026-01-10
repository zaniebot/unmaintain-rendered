```yaml
number: 10564
title: Select more rules for typeshed in the ecosystem checks
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: ecosystem-typeshed
created_at: 2024-03-25T10:18:02Z
updated_at: 2024-03-25T14:22:43Z
url: https://github.com/astral-sh/ruff/pull/10564
synced_at: 2026-01-10T22:47:02Z
```

# Select more rules for typeshed in the ecosystem checks

---

_Pull request opened by @AlexWaygood on 2024-03-25 10:18_

## Summary

This PR selects more rules for typeshed in the ecosystem checks, bringing the configuration slightly closer to the (somewhat complex) ruff config typeshed now uses in CI for `.pyi` files: https://github.com/python/typeshed/blob/f337eb8a5149a05528d19605b7ddd0bc9b5125e4/pyproject.toml#L26-L149. In particular, having `F` selected would have prevented https://github.com/astral-sh/ruff/issues/10509 from occuring, as the regression would have showed up in the ecosystem comment before I merged the PR that caused the regression (https://github.com/astral-sh/ruff/pull/10341).


---

_Comment by @github-actions[bot] on 2024-03-25 10:36_

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

_Label `ci` added by @MichaReiser on 2024-03-25 14:01_

---

_@MichaReiser approved on 2024-03-25 14:01_

---

_Merged by @AlexWaygood on 2024-03-25 14:04_

---

_Closed by @AlexWaygood on 2024-03-25 14:04_

---

_Branch deleted on 2024-03-25 14:04_

---

_Comment by @zanieb on 2024-03-25 14:21_

No harm to just select ALL on more repositories too.

---

_Comment by @AlexWaygood on 2024-03-25 14:22_

> No harm to just select ALL on more repositories too.

I wondered about it, but there's a risk that some categories (e.g. flake8-bugbear) that only really make sense for `.py` files might be quite noisy if we ran them on typeshed in the ecosystem checks.

---
