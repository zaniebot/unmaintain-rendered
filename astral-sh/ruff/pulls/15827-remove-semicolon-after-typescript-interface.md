```yaml
number: 15827
title: Remove semicolon after TypeScript interface definition
type: pull_request
state: merged
author: leotaku
labels:
  - bug
assignees: []
merged: true
base: main
head: remove-semicolon
created_at: 2025-01-30T13:11:45Z
updated_at: 2025-01-30T15:10:17Z
url: https://github.com/astral-sh/ruff/pull/15827
synced_at: 2026-01-10T19:57:22Z
```

# Remove semicolon after TypeScript interface definition

---

_Pull request opened by @leotaku on 2025-01-30 13:11_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR removes a trailing semicolon after an interface definition in the custom TypeScript section of `ruff_wasm`.  Currently, this semicolon triggers the error "TS1036: Statements are not allowed in ambient contexts" when including the file and compiling with e.g `tsc`.

## Test Plan

I made the change, ran `wasm-pack` and copied the generated directory manually to my `node_modules` folder. I then compiled a file importing `@astral-sh/ruff-wasm-web` again and confirmed that the compilation error was gone.


---

_Renamed from "[`ruff_wasm`] Remove semicolon after TypeScript interface definition" to "Remove semicolon after TypeScript interface definition" by @leotaku on 2025-01-30 13:17_

---

_Comment by @github-actions[bot] on 2025-01-30 13:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2025-01-30 15:09_

---

_Label `internal` added by @dhruvmanila on 2025-01-30 15:09_

---

_Label `internal` removed by @dhruvmanila on 2025-01-30 15:10_

---

_Label `bug` added by @dhruvmanila on 2025-01-30 15:10_

---

_Merged by @dhruvmanila on 2025-01-30 15:10_

---

_Closed by @dhruvmanila on 2025-01-30 15:10_

---
