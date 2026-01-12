```yaml
number: 17795
title: "[red-knot] Distinguish fully static protocols from non-fully-static protocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/fully-static-protocols-2
created_at: 2025-05-02T18:35:00Z
updated_at: 2025-05-03T10:12:25Z
url: https://github.com/astral-sh/ruff/pull/17795
synced_at: 2026-01-12T15:56:05Z
```

# [red-knot] Distinguish fully static protocols from non-fully-static protocols

---

_@AlexWaygood_

## Summary

This PR teaches red-knot to differentiate between fully-static and non-fully-static protocol types. This fixes several TODOs in the test suite, but the main motivation here is that it's an excuse to start collecting the types of protocol members as well as the names of those members -- these will be very important for a proper implementation of assignability and subtyping down the road.

This is best reviewed commit by commit:
1. The first commit implements the substantive changes of this PR
2. The second commit doesn't implement any substantive changes; it _only_ moves the protocol-related infrastructure out of `class.rs` and into a new submodule
3. The third commit just cleans up some documentation.

## Test Plan

Existing mdtest assertions updated, and some new ones added


---

_Label `red-knot` added by @AlexWaygood on 2025-05-02 18:35_

---

_Comment by @github-actions[bot] on 2025-05-02 18:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:1 on 2025-05-02 19:22_

the new tests in this PR were part of me debugging an error surfaced by primer on an earlier version of this PR. I wasn't sure whether the bug was in the support for protocols or the support for generics. It was in the support for protocols! But, we don't have enough tests for the interactions between generics and protocols, so I left them in the PR anyway.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/protocol_class.rs`:93 on 2025-05-02 19:23_

I'd like for `ProtocolInterface` to wrap an `FxOrderMap<Name, ProtocolMemberData<'db>>` rather than a `Box<[ProtocolMember<'db>]>` -- then this wouldn't need to be `O(n^2)` in complexity. But `FxOrderMap` needs to have `salsa::Update` derived for it, which can only be done from the Salsa repo due to the Orphan rule for traits (thanks @MichaReiser for helping me debug that earlier!)

---

_@AlexWaygood reviewed on 2025-05-02 19:23_

---

_Marked ready for review by @AlexWaygood on 2025-05-02 19:24_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-02 19:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-02 19:24_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-02 19:24_

---

_Comment by @AlexWaygood on 2025-05-02 19:30_

the primer results are just a bunch of unions that we no longer simplify into a single type, because we see that there is an assignability relation but we no longer recognise a subtype relation.

---

_@MichaReiser reviewed on 2025-05-02 19:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/protocol_class.rs`:93 on 2025-05-02 19:33_

Could we use a `BTreeMap` isntead of an `FxOrderedMap`. Salsa implements `Update` for BTeeMap and BTreeSet (see [here](https://github.com/salsa-rs/salsa/blob/2c041763b75c774916b8912a50035578e6fd59a2/src/update.rs#L343-L351) and `BTreeMap` has nice `difference` methods which I think `OrderedMap` doesn't have

---

_@AlexWaygood reviewed on 2025-05-02 19:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/protocol_class.rs`:93 on 2025-05-02 19:35_

Oh, and we want it to be sorted anyway, so a `BTreeMap` makes a lot of sense. Of course, thanks!

---

_@MichaReiser reviewed on 2025-05-02 19:38_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/protocol_class.rs`:93 on 2025-05-02 19:38_

Oh, it doesn't have a difference method. Is that only available for sets. But you can compare the `keys` iterator respectively. 

Another possibility is to keep using `Box<[...]>`, but sort the entries before returning (I would probably use some wrapper type there). Unless we have operations where we need to get an element by name.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/protocol_class.rs`:93 on 2025-05-02 19:44_

> Unless we have operations where we need to get an element by name.

we do, and we'll have more in the future

---

_@AlexWaygood reviewed on 2025-05-02 19:44_

---

_@carljm approved on 2025-05-02 22:48_

Looks great!

---

_Merged by @AlexWaygood on 2025-05-03 10:12_

---

_Closed by @AlexWaygood on 2025-05-03 10:12_

---

_Branch deleted on 2025-05-03 10:12_

---
