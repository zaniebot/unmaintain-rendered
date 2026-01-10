```yaml
number: 17109
title: "[red-knot] Playground improvements"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-preserve-editor-state
created_at: 2025-04-01T08:00:00Z
updated_at: 2025-04-01T08:04:53Z
url: https://github.com/astral-sh/ruff/pull/17109
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Playground improvements

---

_Pull request opened by @MichaReiser on 2025-04-01 08:00_

## Summary

A few smaller editor improvements that felt worth pulling out of my other feature PRs:

* Load the `Editor` lazily: This allows splitting the entire monaco javascript into a separate async bundle, drastically reducing the size of the `index.js`
* Fix the name of `to_range` and `text_range` to the more idiomatic js names `toRange` and `textRange`
* Use one indexed values for `Position::line` and `Position::column`, which is the same as monaco (reduces the need for `+1` and `-1` operations spread all over the place)
* Preserve the editor state when navigating between tabs. This ensures that selections are preserved even when switching between tabs.
* Stop the default handling of the `Enter` key press event when renaming a file because it resulted in adding a newline in the editor




---

_Label `playground` added by @MichaReiser on 2025-04-01 08:00_

---

_Label `red-knot` added by @MichaReiser on 2025-04-01 08:00_

---

_Comment by @github-actions[bot] on 2025-04-01 08:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-04-01 08:04_

---

_Review requested from @carljm by @MichaReiser on 2025-04-01 08:04_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-01 08:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-01 08:04_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-01 08:04_

---

_Merged by @MichaReiser on 2025-04-01 08:04_

---

_Closed by @MichaReiser on 2025-04-01 08:04_

---

_Branch deleted on 2025-04-01 08:04_

---
