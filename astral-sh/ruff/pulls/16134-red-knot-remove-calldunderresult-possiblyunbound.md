```yaml
number: 16134
title: "[red-knot] Remove `CallDunderResult::PossiblyUnbound` variant"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
base: main
head: micha/remove-dunder-call-possibly-unbound
created_at: 2025-02-13T09:04:35Z
updated_at: 2025-02-14T07:14:46Z
url: https://github.com/astral-sh/ruff/pull/16134
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Remove `CallDunderResult::PossiblyUnbound` variant

---

_@MichaReiser_

## Summary

This PR removes the `CallDunderResult::PossiblyUnbound` variant and instead uses the `CallOutcome` 
variant instead.

The motivation for this change is that a possibly unbound dunder could be represented in two ways before this change:

* `CallDunderResult::PossiblyUnbound`
* `CallDunderResult::CallOutcome(CallOutcome::PossiblyUnboundDunder)` 

We only ever created the first variant (to my knowledge) but this is hard to tell by just reading the downstream code. 

We can't remove `CallOutcome::PossiblyUnboundDunder` because it is used to emit a diagnostic in `CallOutcome::return_type_result`. That's why `CallDunderResult::PossiblyUnbound` had to go ;)

There's a second commit that renames `CallDunderResult` to `CallDunderOutcome`

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-02-13 09:04_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-13 09:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-13 09:04_

---

_Label `red-knot` added by @MichaReiser on 2025-02-13 09:05_

---

_@carljm approved on 2025-02-14 00:25_

---

_Comment by @MichaReiser on 2025-02-14 07:14_

I might actually decide to go the other way round :) I'll reopen if my other refactor doesn't pan out.

---

_Closed by @MichaReiser on 2025-02-14 07:14_

---
