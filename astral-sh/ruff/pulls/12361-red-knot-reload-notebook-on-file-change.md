```yaml
number: 12361
title: "[red-knot] Reload notebook on file change"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: reload-notebook-on-change
created_at: 2024-07-17T12:19:20Z
updated_at: 2024-07-17T12:33:03Z
url: https://github.com/astral-sh/ruff/pull/12361
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Reload notebook on file change

---

_@MichaReiser_

## Summary

During a VC with Alex I noticed that I forgot to add a dependency on the `File`'s revision when reading notebooks. 
The implication of this is that a notebook file never gets reloaded. 

This PR adds a new `File::read_to_notebook` method similar to `File::read_to_string` that correctly adds the `revision` dependency.

## Test Plan

I verified that adding an unknown import to a notebook after starting Red Knot triggers a re-run and that it emits a diagnostic for the import


---

_Review requested from @carljm by @MichaReiser on 2024-07-17 12:19_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-17 12:19_

---

_Label `red-knot` added by @MichaReiser on 2024-07-17 12:19_

---

_Merged by @MichaReiser on 2024-07-17 12:23_

---

_Closed by @MichaReiser on 2024-07-17 12:23_

---

_Branch deleted on 2024-07-17 12:23_

---

_Comment by @github-actions[bot] on 2024-07-17 12:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
