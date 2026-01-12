```yaml
number: 11003
title: "Improve display of rules in `--show-settings`"
type: pull_request
state: merged
author: zanieb
labels:
  - linter
assignees: []
merged: true
base: main
head: zb/show-rule-code
created_at: 2024-04-17T19:54:24Z
updated_at: 2024-04-17T20:22:54Z
url: https://github.com/astral-sh/ruff/pull/11003
synced_at: 2026-01-12T15:55:34Z
```

# Improve display of rules in `--show-settings`

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/11002

---

_Label `linter` added by @zanieb on 2024-04-17 19:54_

---

_@charliermarsh reviewed on 2024-04-17 20:06_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/registry/rule_set.rs`:294 on 2024-04-17 20:06_

Should we change this to use kebab-case while we’re here?

---

_@zanieb reviewed on 2024-04-17 20:09_

---

_Review comment by @zanieb on `crates/ruff_linter/src/registry/rule_set.rs`:294 on 2024-04-17 20:09_

That's what `.as_ref()` does :)

---

_Renamed from "Display rule codes in `--show-settings`" to "Improve display of rules in `--show-settings`" by @zanieb on 2024-04-17 20:10_

---

_@charliermarsh approved on 2024-04-17 20:11_

Sweet!

---

_Merged by @zanieb on 2024-04-17 20:18_

---

_Closed by @zanieb on 2024-04-17 20:18_

---

_Branch deleted on 2024-04-17 20:18_

---

_Comment by @github-actions[bot] on 2024-04-17 20:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
