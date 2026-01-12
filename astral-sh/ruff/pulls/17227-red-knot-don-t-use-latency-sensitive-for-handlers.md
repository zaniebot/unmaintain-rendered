```yaml
number: 17227
title: "[red-knot] Don't use latency-sensitive for handlers"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/lsp-not-sensitive
created_at: 2025-04-06T08:00:49Z
updated_at: 2025-04-08T06:33:32Z
url: https://github.com/astral-sh/ruff/pull/17227
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Don't use latency-sensitive for handlers

---

_@MichaReiser_

## Summary

The priority latency-sensitive is reserved for actions that need to run immediately because they would otherwise block the user's action. An example of this is a format request. VS code blocks the editor until the save action is complete. That's why formatting a document is very sensitive to delays and it's important that we always have a worker thread available to run a format request *immediately*. Another example are code completions, where it's important that they appear immediately when the user types.

On the other hand, showing diagnostics, hover, or inlay hints has high priority but users are used that the editor takes a few ms to compute the overlay. 
Computing this information can also be expensive (e.g. find all references), blocking the worker for quiet some time (a few 100ms). That's why it's important
that those requests don't clog the sensitive worker threads.



---

_Review requested from @carljm by @MichaReiser on 2025-04-06 08:00_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-06 08:00_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-06 08:00_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-06 08:00_

---

_Label `server` added by @MichaReiser on 2025-04-06 08:00_

---

_Label `red-knot` added by @MichaReiser on 2025-04-06 08:00_

---

_Comment by @github-actions[bot] on 2025-04-06 08:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @dcreager removed by @MichaReiser on 2025-04-06 08:05_

---

_Review request for @carljm removed by @MichaReiser on 2025-04-06 08:05_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-04-06 08:05_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-04-06 08:05_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-06 08:05_

---

_Comment by @github-actions[bot] on 2025-04-06 08:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2025-04-07 19:49_

Makes sense

---

_Merged by @MichaReiser on 2025-04-08 06:33_

---

_Closed by @MichaReiser on 2025-04-08 06:33_

---

_Branch deleted on 2025-04-08 06:33_

---
