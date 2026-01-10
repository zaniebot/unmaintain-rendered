```yaml
number: 17520
title: "[red-knot] Fix offset handling in playground for 2-code-point UTF16 characters"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/knot-playground-utf16
created_at: 2025-04-21T11:45:43Z
updated_at: 2025-04-27T10:46:21Z
url: https://github.com/astral-sh/ruff/pull/17520
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Fix offset handling in playground for 2-code-point UTF16 characters

---

_Pull request opened by @MichaReiser on 2025-04-21 11:45_

## Summary

This PR fixes the squiggly line for https://types.ruff.rs/af810c33-1df7-49a1-b86d-c735ad0964c5 where one range contains characters (emoji) that are two UTF-16 codepoints wide. 

This PR extends the wasm API with an optional `PositionEncoding` argument that allows consumers to specify if they want the positions to use UTF8 (byte offsets), UTF16 (code units) or UTF32 (unicode scalar value) offsets for characters. The playground initializes the workspace with UTF16 because that's what JS uses.

## Test Plan

<img width="1987" alt="Screenshot 2025-04-21 at 13 45 38" src="https://github.com/user-attachments/assets/ce0dbf95-2976-42e3-b684-da2114f615aa" />



---

_Review requested from @carljm by @MichaReiser on 2025-04-21 11:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-21 11:45_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-21 11:45_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-21 11:45_

---

_Comment by @github-actions[bot] on 2025-04-21 11:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `playground` added by @MichaReiser on 2025-04-21 11:56_

---

_Label `red-knot` added by @MichaReiser on 2025-04-21 11:56_

---

_Converted to draft by @MichaReiser on 2025-04-21 11:56_

---

_Review request for @dcreager removed by @MichaReiser on 2025-04-23 13:31_

---

_Review request for @carljm removed by @MichaReiser on 2025-04-23 13:31_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-04-23 13:31_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-04-23 13:31_

---

_Marked ready for review by @MichaReiser on 2025-04-24 07:46_

---

_Comment by @github-actions[bot] on 2025-04-24 07:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila approved on 2025-04-25 18:08_

Nice!

---

_Merged by @MichaReiser on 2025-04-27 10:44_

---

_Closed by @MichaReiser on 2025-04-27 10:44_

---

_Branch deleted on 2025-04-27 10:44_

---
