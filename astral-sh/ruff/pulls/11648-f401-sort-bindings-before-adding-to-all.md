```yaml
number: 11648
title: F401 sort bindings before adding to __all__
type: pull_request
state: merged
author: plredmond
labels:
  - bug
assignees: []
merged: true
base: main
head: ruff.f401.nondet2
created_at: 2024-05-31T19:27:37Z
updated_at: 2024-05-31T20:38:57Z
url: https://github.com/astral-sh/ruff/pull/11648
synced_at: 2026-01-10T21:56:00Z
```

# F401 sort bindings before adding to __all__

---

_Pull request opened by @plredmond on 2024-05-31 19:27_

Sort the binding IDs before passing them to the add-to-`__all__` function to address #11619.

---

_Comment by @plredmond on 2024-05-31 19:29_

I ended up sorting the slice rather than the result of the iter/map because that saves a copy. I made the function argument `&mut` because I figure it's only used in this module. Might be controversial.

---

_Comment by @github-actions[bot] on 2024-05-31 19:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-05-31 20:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:333 on 2024-05-31 20:21_

Maybe we should just pass in the `Vec` here, so the ownership is clearer? Then it can be `mut imports: Vec<&ImportBinding>` and callers don't need to worry about the mutation.

---

_@charliermarsh reviewed on 2024-05-31 20:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:333 on 2024-05-31 20:25_

Tweaking this quickly so I can include in the release.

---

_Label `bug` added by @charliermarsh on 2024-05-31 20:25_

---

_@charliermarsh approved on 2024-05-31 20:25_

---

_Merged by @charliermarsh on 2024-05-31 20:29_

---

_Closed by @charliermarsh on 2024-05-31 20:29_

---

_Branch deleted on 2024-05-31 20:29_

---

_@plredmond reviewed on 2024-05-31 20:38_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:333 on 2024-05-31 20:38_

Ok! Thanks for explaining

---
