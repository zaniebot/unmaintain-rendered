```yaml
number: 21188
title: "[`ruff`] Add capabilities check for `clear_diagnostics_for_document`"
type: pull_request
state: open
author: TaKO8Ki
labels: []
assignees: []
base: main
head: add-capabilities-check-for-clear_diagnostics_for_document
created_at: 2025-11-01T19:16:07Z
updated_at: 2025-11-03T14:50:50Z
url: https://github.com/astral-sh/ruff/pull/21188
synced_at: 2026-01-10T16:53:55Z
```

# [`ruff`] Add capabilities check for `clear_diagnostics_for_document`

---

_Pull request opened by @TaKO8Ki on 2025-11-01 19:16_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20784 cc: @MichaReiser 

This is a follow up to https://github.com/astral-sh/ruff/pull/21105.

#21105 affected Notebook, so in this pull request I just fixed `clear_diagnostics_for_document` that is not related to Notebook.

## Test Plan

<!-- How was it tested? -->

![output2](https://github.com/user-attachments/assets/78bbb4fd-4217-46a4-b4d0-743675533d70)


---

_@TaKO8Ki reviewed on 2025-11-01 19:19_

---

_Review comment by @TaKO8Ki on `crates/ruff_server/src/server/api/diagnostics.rs`:48 on 2025-11-01 19:19_

This change only affects `DidClose`. For Notebook, there is `DidCloseNotebook`, so it should not affect Notebook.

https://github.com/astral-sh/ruff/blob/f9a461a6db2780f51b81ad11959118d637c3b95e/crates/ruff_server/src/server/api/notifications/did_close.rs#L14

https://github.com/astral-sh/ruff/blob/f9a461a6db2780f51b81ad11959118d637c3b95e/crates/ruff_server/src/server/api/notifications/did_close_notebook.rs#L13

---

_Comment by @github-actions[bot] on 2025-11-01 19:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-11-03 14:50_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/notifications/did_close.rs`:30 on 2025-11-03 14:50_

I'm not sure this is correct. This is probably a bit surprising but notebook cells are text documents and the LSP sends close document requests for LSP cells. 

Would you mind extending your test plan to cover 
* Closing a notebook that has diagnostics
* Removing a cell that contains diagnostics

---
