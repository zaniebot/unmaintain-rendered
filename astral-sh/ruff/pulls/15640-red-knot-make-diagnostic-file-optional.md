```yaml
number: 15640
title: "[red-knot] Make `Diagnostic::file` optional"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: micha/diagnostic-optional-file
created_at: 2025-01-21T13:39:40Z
updated_at: 2025-01-23T09:43:17Z
url: https://github.com/astral-sh/ruff/pull/15640
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Make `Diagnostic::file` optional

---

_Pull request opened by @MichaReiser on 2025-01-21 13:39_

## Summary

I'm working on adding a rules table to Red Knot's configuration. 
What's special about rules is that we intentionally want to defer validating
the rule names so that we can *warn* about incorrect rule names rather than just abort. 
By warning I mean we'd create a diagnostic. This is a functionality that has been requested for ruff (https://github.com/astral-sh/ruff/issues/14443).

The one problem I ran into when creating the diagnostic is that we don't always 
have a file. For example, the rule could have been enabled on the CLI. 
(actually, I didn't run into this yet but we will once we add `--ignore` and `--error` and `--warn` CLI flags).

This PR makes `file` an `Diagnostic` an `Option` to account for the case where we don't have a file.  Doing so has the disadvantage that it's now technically possible for a diagnostic to have a range but not an associated `File`. I intentionally decided not to design the "perfect" solution because the API is likely to change with @BurntSushi's work
on Diagnostics. 

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2025-01-21 13:47_

---

_Label `diagnostics` added by @MichaReiser on 2025-01-21 13:47_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-01-21 13:52_

---

_Marked ready for review by @MichaReiser on 2025-01-21 13:53_

---

_Review requested from @carljm by @MichaReiser on 2025-01-21 13:53_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-21 13:53_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-21 13:53_

---

_Comment by @github-actions[bot] on 2025-01-21 14:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@BurntSushi approved on 2025-01-21 14:15_

This makes sense to me. And I like this change because it will force me to confront it. :-) Thank you!

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic.rs`:161 on 2025-01-21 15:45_

is it worth adding a doc-comment explaining why `file` might sometimes be `None`? Your explanation in the PR summary is very good!

---

_@AlexWaygood approved on 2025-01-21 15:45_

---

_Merged by @MichaReiser on 2025-01-23 09:43_

---

_Closed by @MichaReiser on 2025-01-23 09:43_

---

_Branch deleted on 2025-01-23 09:43_

---
