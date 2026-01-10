```yaml
number: 14914
title: "Support `lint:<rule>` in mdtests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fix-mdtest-assertion
created_at: 2024-12-11T13:16:00Z
updated_at: 2024-12-11T13:37:15Z
url: https://github.com/astral-sh/ruff/pull/14914
synced_at: 2026-01-10T20:42:27Z
```

# Support `lint:<rule>` in mdtests

---

_Pull request opened by @MichaReiser on 2024-12-11 13:16_

## Summary

Fixes a small scoping issue in `DiagnosticId::matches`

Note: I don't think we should use `lint:id` in mdtests just yet. I worry that it could lead to many unnecessary churns if we decide **not** to use `lint:<id>` as the format (e.g., `lint/id`). 

The reason why users even see `lint:<rule>` is because the mdtest framework uses the diagnostic infrastructure

Closes #14910

## Test Plan

Added tests


---

_Review requested from @carljm by @MichaReiser on 2024-12-11 13:16_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-11 13:16_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-11 13:16_

---

_Label `red-knot` added by @MichaReiser on 2024-12-11 13:17_

---

_Comment by @github-actions[bot] on 2024-12-11 13:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-12-11 13:25_

> Note: I don't think we should use `lint:id` in doctests just yet.

Do you mean mdtests? If so, I think I'd prefer to fix this at the other end, and make it so that when the test fails, mdtest just prints the message that we want to be added to the Markdown file. That way I can just copy and paste the message straight from my terminal into the Markdown file, rather than having to fiddle about and remove the `lint:` prefix from the error code. It's much more ergonomic

---

_Comment by @MichaReiser on 2024-12-11 13:37_

I planned to push it to this PR but didn't realize I was on main.. and branch protection was off, oops. I re-enabled branch protection. 

---

_Merged by @MichaReiser on 2024-12-11 13:37_

---

_Closed by @MichaReiser on 2024-12-11 13:37_

---

_Branch deleted on 2024-12-11 13:37_

---
