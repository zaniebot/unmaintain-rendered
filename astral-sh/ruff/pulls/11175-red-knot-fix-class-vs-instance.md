```yaml
number: 11175
title: "[red-knot] fix class vs instance"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/class-vs-instance
created_at: 2024-04-27T14:40:19Z
updated_at: 2024-04-27T15:09:03Z
url: https://github.com/astral-sh/ruff/pull/11175
synced_at: 2026-01-12T15:55:36Z
```

# [red-knot] fix class vs instance

---

_@carljm_

## Summary

Clarify the type of an exact class object vs the type of instances of that class.

## Test Plan

cargo test


---

_Label `internal` added by @carljm on 2024-04-27 14:40_

---

_Review requested from @MichaReiser by @carljm on 2024-04-27 14:40_

---

_Review requested from @AlexWaygood by @carljm on 2024-04-27 14:40_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types.rs`:359 on 2024-04-27 14:43_

Previous proposed spellings for "exactly the `str` class" (and not a subclass) have included:
- `Literal[str]`: https://mail.python.org/archives/list/typing-sig@python.org/message/FPQH5ZHFLHGB7PMWEUHTB4FOK5O6MN6Q/
- `TypeForm[str]`: https://mail.python.org/archives/list/typing-sig@python.org/message/O6AYFN7WN623W5EJ7LULSD4LBA2M3IOA/

I'd be happy with either `Exact[str]` or `Literal[str]`. I weakly prefer `Literal[str]`, because `Literal` elsewhere already specifies "exactly this value I'm specifying here, and no subtype or supertype of it"; and it also means that we can reuse an existing symbol rather than having to add a new special form (even if it's an internal-only special form)

---

_@AlexWaygood reviewed on 2024-04-27 14:43_

---

_@carljm reviewed on 2024-04-27 14:47_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:359 on 2024-04-27 14:47_

I like `Literal[...]` for this, I'll switch to that.

---

_@AlexWaygood approved on 2024-04-27 14:48_

---

_Comment by @github-actions[bot] on 2024-04-27 14:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-04-27 14:57_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types.rs`:24 on 2024-04-27 14:57_

This isn't really related to your PR, but I'm kinda leery of thinking of this as a "type", FWIW (I know pyright does this, but I don't like it much). Whether a symbol is bound or not in a given scope is obviously important for us to track, but it doesn't feel like it has much to do with the inferred _type_ of the symbol to me

---

_@carljm reviewed on 2024-04-27 14:59_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:24 on 2024-04-27 14:59_

I keep going back and forth on this, I see arguments both ways. Like you say, it's not related to this PR, and nothing is using this yet anyway, so there's not an urgency to decide here, but we should discuss.

---

_Merged by @carljm on 2024-04-27 15:09_

---

_Closed by @carljm on 2024-04-27 15:09_

---

_Branch deleted on 2024-04-27 15:09_

---
