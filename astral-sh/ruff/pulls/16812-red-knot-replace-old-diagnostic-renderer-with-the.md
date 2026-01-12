```yaml
number: 16812
title: "[red-knot] replace old diagnostic renderer with the new"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/rug-pull
created_at: 2025-03-17T15:24:34Z
updated_at: 2025-03-17T16:46:51Z
url: https://github.com/astral-sh/ruff/pull/16812
synced_at: 2026-01-12T15:55:58Z
```

# [red-knot] replace old diagnostic renderer with the new

---

_@BurntSushi_

This PR "rug pulls" the old renderer with the new.

The purpose here is to separate this change from a broader refactor
of moving Red Knot over to the new diagnostic data model. This also
enables us to actually start *using* the new renderer.

My initial goal here was to minimize the changes to the output format,
perhaps to no changes at all. While we *could* do that, I'm not sure
it's worth the effort. I think further customization of the output
should come in the form of making Red Knot use the more expressive
`Diagnostic` API. I tried to contextualize the changes that were made
to the output format in the last commit with all of the snapshot
changes.

Closes #16808


---

_Review requested from @carljm by @BurntSushi on 2025-03-17 15:24_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-03-17 15:24_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-03-17 15:24_

---

_Review requested from @sharkdp by @BurntSushi on 2025-03-17 15:24_

---

_Review requested from @dcreager by @BurntSushi on 2025-03-17 15:24_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-17 15:25_

---

_Label `diagnostics` added by @AlexWaygood on 2025-03-17 15:26_

---

_Comment by @github-actions[bot] on 2025-03-17 15:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-17 16:42_

---

_@MichaReiser approved on 2025-03-17 16:43_

---

_Comment by @BurntSushi on 2025-03-17 16:46_

Bringing this in. If anyone else reviews this, I'd be happy to file a follow-up PR to address comments!

---

_Merged by @BurntSushi on 2025-03-17 16:46_

---

_Closed by @BurntSushi on 2025-03-17 16:46_

---

_Branch deleted on 2025-03-17 16:46_

---
