---
number: 14141
title: "[red-knot] consolidate detection of cyclic class definitions"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-11-06T21:33:02Z
updated_at: 2024-11-08T22:17:57Z
url: https://github.com/astral-sh/ruff/issues/14141
synced_at: 2026-01-10T01:22:54Z
---

# [red-knot] consolidate detection of cyclic class definitions

---

_Issue opened by @carljm on 2024-11-06 21:33_

Currently both MRO resolution and metaclass resolution separately detect cycles, but we emit the diagnostic only from MRO resolution and ignore it from metaclass resolution. This is inefficient both in code to maintain and runtime cost.

Since detection of cyclic class definitions in the MRO case is already implemented as a standalone function, we can instead hoist the call to that function out to `TypeInferenceBuilder::check_class_definitions`, before resolving MRO or metaclass. If the class definition is cyclic, we can specify an `Unknown` result for the class' MRO and metaclass and avoid ever calculating them. Then MRO and metaclass resolution can assume non-cyclic as an invariant.

One thing we may have to guard against here is if the MRO or metaclass is accessed before we've done full inference of the scope the class is defined in.

---

_Label `red-knot` added by @carljm on 2024-11-06 21:33_

---

_Referenced in [astral-sh/ruff#14120](../../astral-sh/ruff/pulls/14120.md) on 2024-11-06 21:33_

---

_Comment by @MichaReiser on 2024-11-06 21:47_

Wouldn't that regress the recovery? I believe we now only replace the cyclic-subgraph with `Unknown` instead of the entire MRO.

---

_Comment by @carljm on 2024-11-06 22:18_

> Wouldn't that regress the recovery? I believe we now only replace the cyclic-subgraph with `Unknown` instead of the entire MRO.

Yes, but I think this doesn't actually matter at all.

---

_Comment by @MichaReiser on 2024-11-06 22:22_

> Yes, but I think this doesn't actually matter at all.

Is that because `Unknown` silences all resulting false positives anyway? 

---

_Comment by @carljm on 2024-11-06 22:32_

> Is that because `Unknown` silences all resulting false positives anyway?

Exactly. It may mean a few more false negatives, but if you have a cyclic class definition it'd be perfectly reasonable to call the entire class `Unknown`; it's really not worth going out of our way for incrementally more precise types on a nonsense/impossible class definition.

---

_Referenced in [astral-sh/ruff#14207](../../astral-sh/ruff/pulls/14207.md) on 2024-11-08 17:49_

---

_Closed by @AlexWaygood on 2024-11-08 22:17_

---
