```yaml
number: 10370
title: "Sort hash maps in `Settings` display"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/set
created_at: 2024-03-12T19:13:40Z
updated_at: 2024-03-18T10:25:49Z
url: https://github.com/astral-sh/ruff/pull/10370
synced_at: 2026-01-12T15:55:32Z
```

# Sort hash maps in `Settings` display

---

_@charliermarsh_

## Summary

We had a report of a test failure on a specific architecture, and looking into it, I think the test assumes that the hash keys are iterated in a specific order. This PR thus adds a variant to our settings display macro specifically for maps and sets. Like `CacheKey`, it sorts the keys when printing.

Closes https://github.com/astral-sh/ruff/issues/10359.


---

_Review requested from @snowsignal by @charliermarsh on 2024-03-12 19:13_

---

_Label `bug` added by @charliermarsh on 2024-03-12 19:13_

---

_@charliermarsh reviewed on 2024-03-12 19:14_

---

_Review comment by @charliermarsh on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:259 on 2024-03-12 19:14_

This is also now more consistent with our array printing.

---

_@charliermarsh reviewed on 2024-03-12 19:14_

---

_Review comment by @charliermarsh on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:320 on 2024-03-12 19:14_

These are sets. We could represent them as `[]` since that's how they're presented in TOML. I don't feel strongly.

---

_@zanieb reviewed on 2024-03-12 19:15_

---

_Review comment by @zanieb on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:320 on 2024-03-12 19:15_

I'd prefer to distinguish them from an empty key/value structure so `[]` seems better.

---

_@charliermarsh reviewed on 2024-03-12 19:15_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/isort/settings.rs`:66 on 2024-03-12 19:15_

I changed these to use `Fx` for consistency with other settings, now that we sort in display anyway.

---

_Review comment by @charliermarsh on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:320 on 2024-03-12 19:16_

Sounds good, although now they're not distinguished from an empty array :)

---

_@charliermarsh reviewed on 2024-03-12 19:16_

---

_@zanieb reviewed on 2024-03-12 19:16_

---

_Review comment by @zanieb on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:320 on 2024-03-12 19:16_

I don't think that it's an array vs a set matters in this context, right?

---

_@zanieb approved on 2024-03-12 19:21_

---

_Comment by @github-actions[bot] on 2024-03-12 19:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-03-12 19:45_

I think the ecosystem check is a false positive? I see that violation on main and on this branch.

---

_Merged by @charliermarsh on 2024-03-12 19:59_

---

_Closed by @charliermarsh on 2024-03-12 19:59_

---

_Branch deleted on 2024-03-12 19:59_

---

_@MichaReiser reviewed on 2024-03-18 10:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/mod.rs`:172 on 2024-03-18 10:25_

Nit: I think it would be good to move out as much code as possible from the macro. we could possibly do this by having a `DisplaySet` and `DisplayMap` struct that we would instantiate here (and call into) rather than implementing the logic inside of the macro. This has the added benefit that the logic can be reused elsewhere

---
