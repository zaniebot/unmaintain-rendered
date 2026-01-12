```yaml
number: 10512
title: "Don't treat annotations as redefinitions in `.pyi` files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/F811
created_at: 2024-03-21T16:07:33Z
updated_at: 2024-03-21T16:22:51Z
url: https://github.com/astral-sh/ruff/pull/10512
synced_at: 2026-01-12T15:55:32Z
```

# Don't treat annotations as redefinitions in `.pyi` files

---

_@charliermarsh_

## Summary

In https://github.com/astral-sh/ruff/pull/10341, we fixed some false positives in `.pyi` files, but introduced others. This PR effectively reverts the change in #10341 and fixes it in a slightly different way. Instead of changing the _bindings_ we generate in the semantic model in `.pyi` files, we instead change how we _resolve_ them.

Closes https://github.com/astral-sh/ruff/issues/10509.


---

_Label `bug` added by @charliermarsh on 2024-03-21 16:07_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-03-21 16:07_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-03-21 16:07_

---

_Review request for @MichaReiser removed by @charliermarsh on 2024-03-21 16:07_

---

_Comment by @AlexWaygood on 2024-03-21 16:10_

Nice, this looks good! Let me just run this branch on typeshed quickly.

---

_@AlexWaygood approved on 2024-03-21 16:15_

Confirmed that it fixes the regressions for typeshed, and doesn't introduce any new ones (with our current config, at least)!

---

_Comment by @github-actions[bot] on 2024-03-21 16:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-03-21 16:22_

---

_Closed by @charliermarsh on 2024-03-21 16:22_

---

_Branch deleted on 2024-03-21 16:22_

---
