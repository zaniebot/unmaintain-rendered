```yaml
number: 10100
title: Refactor trailing comma rule into explicit check and state update code
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-trailing-comma-rule
created_at: 2024-02-23T16:40:22Z
updated_at: 2024-02-23T16:56:06Z
url: https://github.com/astral-sh/ruff/pull/10100
synced_at: 2026-01-12T15:55:31Z
```

# Refactor trailing comma rule into explicit check and state update code

---

_@MichaReiser_


## Summary

This PR extracts the diagnostic generation code inside the `trailing_commas` rule into its own function to:

* Separate the state update logic from the actual checking logic
* To make it explicit that the rule adds at most one diagnostic per token

I also renamed the `r#type` field to `ty` to avoid the need for the escape. Using `ty` for `type` isn't uncommon in the rust ecosystem.


## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-02-23 16:43_

---

_Marked ready for review by @MichaReiser on 2024-02-23 16:49_

---

_Comment by @github-actions[bot] on 2024-02-23 16:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-02-23 16:56_

---

_Closed by @MichaReiser on 2024-02-23 16:56_

---

_Branch deleted on 2024-02-23 16:56_

---
