```yaml
number: 14936
title: "[red-knot] Add explicit TODO branches for many typing special forms and qualifiers"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/special-form-todos
created_at: 2024-12-12T13:56:16Z
updated_at: 2024-12-12T17:59:32Z
url: https://github.com/astral-sh/ruff/pull/14936
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Add explicit TODO branches for many typing special forms and qualifiers

---

_@AlexWaygood_

## Summary

The `typing` module has many special forms, aliases and type qualifiers. It's going to take us a while to work through suppport for all of them, but in the meantime we want to keep false positives to a minimum if we're analyzing code that uses one of these features that we don't yet support. This PR therefore adds explicit TODO branches for many of the special forms and type qualifiers that we don't yet support, as well as some very minimal functionality such as an understanding of whether or not a special form is subscriptable or can be used as a base class.

## Test Plan

Several new mdtests added.

---

_Label `red-knot` added by @AlexWaygood on 2024-12-12 13:56_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-12 13:56_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-12 13:56_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-12 13:56_

---

_Comment by @github-actions[bot] on 2024-12-12 14:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/mro.rs`:389 on 2024-12-12 17:37_

Hmmm... ok :) I guess this is perfectly accurate, but I'm tempted to say we should make the mapping explicit.

---

_@carljm approved on 2024-12-12 17:38_

This is awesome, thank you!!

---

_@AlexWaygood reviewed on 2024-12-12 17:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:389 on 2024-12-12 17:41_

ugh fiiiiiine

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/mro.rs`:389 on 2024-12-12 17:42_

I'm also totally fine with not doing it, if you don't think it's worth it :)

---

_@carljm reviewed on 2024-12-12 17:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:389 on 2024-12-12 17:43_

nah I think you're right that this is probably "more correct", I was just being lazy 😆

---

_@AlexWaygood reviewed on 2024-12-12 17:43_

---

_Merged by @AlexWaygood on 2024-12-12 17:57_

---

_Closed by @AlexWaygood on 2024-12-12 17:57_

---

_Branch deleted on 2024-12-12 17:57_

---
