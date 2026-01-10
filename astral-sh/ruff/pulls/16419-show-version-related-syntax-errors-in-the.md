```yaml
number: 16419
title: Show version-related syntax errors in the playground
type: pull_request
state: merged
author: ntBre
labels:
  - playground
assignees: []
merged: true
base: main
head: brent/syntax-errors-playground
created_at: 2025-02-27T17:03:27Z
updated_at: 2025-02-27T18:28:39Z
url: https://github.com/astral-sh/ruff/pull/16419
synced_at: 2026-01-10T19:49:01Z
```

# Show version-related syntax errors in the playground

---

_Pull request opened by @ntBre on 2025-02-27 17:03_

## Summary

Fixes part of https://github.com/astral-sh/ruff/issues/16417 by converting `unsupported_syntax_errors` into playground diagnostics.

## Test Plan

A new `ruff_wasm` test, plus trying out the playground locally:

Default settings:
![image](https://github.com/user-attachments/assets/94377ab5-4d4c-44d3-ae63-fe328a53e083)

`target-version = "py310"`:
![image](https://github.com/user-attachments/assets/51c312ce-70e7-43d3-b6ba-098f2750cb28)



---

_Label `playground` added by @ntBre on 2025-02-27 17:03_

---

_Comment by @github-actions[bot] on 2025-02-27 17:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-02-27 18:18_

---

_Comment by @MichaReiser on 2025-02-27 18:21_

It would be good to trigger a manual playground deployment once this is merged (https://github.com/astral-sh/ruff/actions/workflows/publish-playground.yml)

---

_Comment by @ntBre on 2025-02-27 18:22_

Sounds good, I was wondering about that. Thanks!

---

_Merged by @ntBre on 2025-02-27 18:28_

---

_Closed by @ntBre on 2025-02-27 18:28_

---

_Branch deleted on 2025-02-27 18:28_

---
