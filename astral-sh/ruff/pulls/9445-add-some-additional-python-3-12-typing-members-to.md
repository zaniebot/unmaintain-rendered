```yaml
number: 9445
title: "Add some additional Python 3.12 typing members to `deprecated-import`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/types
created_at: 2024-01-09T05:47:52Z
updated_at: 2024-01-09T17:56:22Z
url: https://github.com/astral-sh/ruff/pull/9445
synced_at: 2026-01-10T22:57:09Z
```

# Add some additional Python 3.12 typing members to `deprecated-import`

---

_Pull request opened by @charliermarsh on 2024-01-09 05:47_

Closes https://github.com/astral-sh/ruff/issues/9443.

---

_Renamed from "Add some additional Python 3.12 typing members to deprecated-import" to "Add some additional Python 3.12 typing members to `deprecated-import`" by @charliermarsh on 2024-01-09 05:47_

---

_Comment by @charliermarsh on 2024-01-09 05:48_

@AlexWaygood - do you mind reviewing this one?

---

_Label `rule` added by @charliermarsh on 2024-01-09 05:48_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:350 on 2024-01-09 05:49_

I just grepped for "New in version 3.12" in the typing documentation.

---

_@charliermarsh reviewed on 2024-01-09 05:49_

---

_Comment by @github-actions[bot] on 2024-01-09 06:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-01-09 07:08_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:350 on 2024-01-09 07:08_

`collections.abc.Buffer` and `types.get_original_bases` are also both new in py312 (and backported via `typing_extensions`) — you could add those as well!

---

_@AlexWaygood reviewed on 2024-01-09 07:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:346 on 2024-01-09 07:22_

I'd also remove both `TypedDict` and `NamedTuple` from the list. We've backported new features to both recently that people might reasonably want to play around with, but won't be able to on py312 or lower:

- https://github.com/python/typing_extensions/commit/4f91502281d748671c7c1dfa26726111853f1342
- https://github.com/python/typing_extensions/commit/0b0166d649cebcb48e7e208ae5da36cfab5965fe



---

_@charliermarsh reviewed on 2024-01-09 15:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:350 on 2024-01-09 15:10_

Awesome, thanks!

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-01-09 16:54_

---

_Comment by @charliermarsh on 2024-01-09 16:54_

\cc @AlexWaygood 

---

_@AlexWaygood reviewed on 2024-01-09 16:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:355 on 2024-01-09 16:59_

Ah, this is in the `types` module in Python 3.12, not `typing`

---

_@AlexWaygood reviewed on 2024-01-09 17:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:355 on 2024-01-09 17:00_

you can blame me for that one, i reviewed and merged the PR adding it...

---

_@charliermarsh reviewed on 2024-01-09 17:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:355 on 2024-01-09 17:42_

Oops, thanks!

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-01-09 17:43_

---

_@AlexWaygood approved on 2024-01-09 17:46_

Looks great, thanks!

---

_Merged by @charliermarsh on 2024-01-09 17:52_

---

_Closed by @charliermarsh on 2024-01-09 17:52_

---

_Branch deleted on 2024-01-09 17:52_

---

_Comment by @charliermarsh on 2024-01-09 17:52_

Thanks for the close read!

---
