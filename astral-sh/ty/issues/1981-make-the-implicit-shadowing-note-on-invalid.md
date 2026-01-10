```yaml
number: 1981
title: "Make the \"implicit shadowing\" note on `invalid-assignment` additional, don't replace the main message"
type: issue
state: closed
author: lucach
labels:
  - good first issue
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-12-17T07:47:01Z
updated_at: 2025-12-29T09:30:50Z
url: https://github.com/astral-sh/ty/issues/1981
synced_at: 2026-01-10T01:56:41Z
```

# Make the "implicit shadowing" note on `invalid-assignment` additional, don't replace the main message

---

_Issue opened by @lucach on 2025-12-17 07:47_

### Summary

In #1556 I suggested to improve the diagnostic range for `invalid-assignment` by moving the range from the target to the expression on the right-hand side. @sharkdp greatly improved this in https://github.com/astral-sh/ruff/pull/21476, but I fear there's one special case for which the changes actually caused a regression.

When `invalid-assignment` warns about implicit shadowing, it is not quite right to blame (only) the RHS. That expression in itself is not faulty. The diagnostic range should either extend to the entire assignment, or just cover the target.

[Minimal example in the Playground](https://play.ty.dev/0ecc746e-20bc-44bf-a885-d44570fd9967)

The relevant code is [here](https://github.com/astral-sh/ruff/blob/b0bc990cbf2a0a75ced52de0d6ba3d51d35072ee/crates/ty_python_semantic/src/types/diagnostic.rs#L2402-L2485), and I would have attempted a fix myself, but I'm not sure where to best treat this special case.

### Version

ad3de4e48

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-17 07:48_

---

_Comment by @lucach on 2025-12-19 08:59_

Thinking about this a bit more, it also doesn't feel ideal that the shadowing diagnostics "suppresses" (at least in the concise diagnostics shown e.g. in the playground) the possible type error.

In the [original example](https://play.ty.dev/0ecc746e-20bc-44bf-a885-d44570fd9967) reported above, there is function shadowing but no type error (indeed, [if you add `Callable[[], None` as a type annotation, ty correctly emits no diagnostics](https://play.ty.dev/4b7f0da7-c3f8-48ec-92b4-b280de3e51cc)).

But there are cases in which **both** shadowing and a type error play a role, but ty reports only one diagnostics about shadowing... which is shadowing (ha!) the "type mismatch" diagnostic. Here is a [simple example](https://play.ty.dev/299f856a-edf3-4e60-b530-6eef9df183db), which [correctly shows up as a type mismatch when adding the annotation](https://play.ty.dev/da7fa0b2-1a86-4d37-a0b5-d8cee17d1a9b).

Given all of this, I wonder if it makes sense to separate shadowing into its own separate diagnostic code, such that when there is both shadowing and a type error, both could be shown, and users could enable/disable shadowing specifically.  

---

_Label `help wanted` added by @carljm on 2025-12-22 23:56_

---

_Label `good first issue` added by @carljm on 2025-12-22 23:56_

---

_Renamed from "Improve diagnostic range for `invalid-assignment` when it is about shadowing" to "Make the "implicit shadowing" note on `invalid-assignment` additional, don't replace the main message" by @carljm on 2025-12-22 23:57_

---

_Comment by @carljm on 2025-12-22 23:59_

Thanks! This "implicit shadowing" diagnostic dates back to very early days of ty, before we had our current diagnostic system. Currently we literally just replace the normal diagnostic message for `invalid-assignment` with a different one, when we detect that the "declared type" is a function or class literal. At the very least, we should instead leave the normal `invalid-assignment` diagnostic message, and tack on "Are you implicitly shadowing a function/class?" as a `note` sub-diagnostic.

---

_Added to milestone `Stable` by @carljm on 2025-12-22 23:59_

---

_Closed by @MichaReiser on 2025-12-29 09:30_

---
