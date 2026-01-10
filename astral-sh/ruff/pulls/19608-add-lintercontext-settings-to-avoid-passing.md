```yaml
number: 19608
title: "Add `LinterContext::settings` to avoid passing separate settings"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/lint-context-settings
created_at: 2025-07-29T00:02:45Z
updated_at: 2025-07-29T12:13:25Z
url: https://github.com/astral-sh/ruff/pull/19608
synced_at: 2026-01-10T17:58:13Z
```

# Add `LinterContext::settings` to avoid passing separate settings

---

_Pull request opened by @ntBre on 2025-07-29 00:02_

Summary
--

I noticed while reviewing #19390 that in `check_tokens` we were still passing
around an extra `LinterSettings`, despite all of the same functions also
receiving a `LintContext` with its own settings.

This PR adds the `LintContext::settings` method and calls that instead of using
the separate `LinterSettings`.

Test Plan
--

Existing tests


---

_Label `internal` added by @ntBre on 2025-07-29 00:02_

---

_Comment by @github-actions[bot] on 2025-07-29 00:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @MichaReiser by @ntBre on 2025-07-29 00:23_

---

_@MichaReiser approved on 2025-07-29 05:18_

---

_Merged by @ntBre on 2025-07-29 12:13_

---

_Closed by @ntBre on 2025-07-29 12:13_

---

_Branch deleted on 2025-07-29 12:13_

---
