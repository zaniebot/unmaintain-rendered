```yaml
number: 11690
title: Make tests aware that py313 is the latest supported Python version
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: latest-version
created_at: 2024-06-02T11:25:46Z
updated_at: 2024-06-02T13:15:42Z
url: https://github.com/astral-sh/ruff/pull/11690
synced_at: 2026-01-10T21:56:00Z
```

# Make tests aware that py313 is the latest supported Python version

---

_Pull request opened by @AlexWaygood on 2024-06-02 11:25_

Working on a PR for #11413, I noticed that the linter tests still thought that py312 was the latest version. This affects which autofixes are displayed in the snapshots for several of the pyupgrade rules.

---

_Label `internal` added by @AlexWaygood on 2024-06-02 11:25_

---

_Comment by @github-actions[bot] on 2024-06-02 11:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:65 on 2024-06-02 12:53_

Nit: I think I would prefer to keep `Self::Py313` and to make the function `const`. To avoid that we forget updating `latest` in the future, I recommend adding a comment in the enum definition after the last variant that tells you to make sure to also change `latest` (Although it may not always be desirable to change latest right away, we may need to add a new variant to test new language features before Ruff fully supports it)

---

_@MichaReiser approved on 2024-06-02 12:53_

---

_Merged by @AlexWaygood on 2024-06-02 13:06_

---

_Closed by @AlexWaygood on 2024-06-02 13:06_

---

_Branch deleted on 2024-06-02 13:06_

---
