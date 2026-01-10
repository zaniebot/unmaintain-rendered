```yaml
number: 15237
title: "Show errors for attempted fixes only when passed `--verbose`"
type: pull_request
state: merged
author: dylwil3
labels:
  - cli
  - diagnostics
assignees: []
merged: true
base: main
head: error-visibility
created_at: 2025-01-03T07:35:50Z
updated_at: 2025-01-03T14:50:18Z
url: https://github.com/astral-sh/ruff/pull/15237
synced_at: 2026-01-10T20:42:27Z
```

# Show errors for attempted fixes only when passed `--verbose`

---

_Pull request opened by @dylwil3 on 2025-01-03 07:35_

The default logging level for diagnostics includes logs written using the `log` crate with level `error`, `warn`, and `info`. An unsuccessful fix attached to a diagnostic via `try_set_fix` or `try_set_optional_fix` was logged at level `error`. Note that the user would see these messages even without passing `--fix`, and possibly also on lines with `noqa` comments.

This PR changes the logging level here to a `debug`. We also found ad-hoc instances of error logging in the implementations of several rules, and have replaced those with either a `debug` or call to `try_set{_optional}_fix`.

Closes #15229

---

_Label `cli` added by @dylwil3 on 2025-01-03 07:35_

---

_Label `diagnostics` added by @dylwil3 on 2025-01-03 07:35_

---

_Comment by @github-actions[bot] on 2025-01-03 07:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-03 07:47_

Thanks

---

_Merged by @dylwil3 on 2025-01-03 14:50_

---

_Closed by @dylwil3 on 2025-01-03 14:50_

---

_Branch deleted on 2025-01-03 14:50_

---
