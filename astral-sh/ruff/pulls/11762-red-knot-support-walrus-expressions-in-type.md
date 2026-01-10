```yaml
number: 11762
title: "[red-knot] support walrus expressions in type inference"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/walrus
created_at: 2024-06-05T20:38:37Z
updated_at: 2024-06-05T21:13:11Z
url: https://github.com/astral-sh/ruff/pull/11762
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] support walrus expressions in type inference

---

_Pull request opened by @carljm on 2024-06-05 20:38_

## Summary

Add support for walrus expressions, both in expression type inference and in symbol definition type inference.

## Test Plan

Added test.


---

_Label `red-knot` added by @carljm on 2024-06-05 20:38_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-05 20:38_

---

_Review requested from @MichaReiser by @carljm on 2024-06-05 20:38_

---

_Renamed from "[red-knot] suppport walrus expressions in type inference" to "[red-knot] support walrus expressions in type inference" by @carljm on 2024-06-05 20:41_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic.rs`:260 on 2024-06-05 20:48_

Should we also immediately mark any symbol defined using a walrus as both defined and used?

---

_@AlexWaygood approved on 2024-06-05 20:48_

---

_Comment by @github-actions[bot] on 2024-06-05 20:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-06-05 20:55_

---

_Review comment by @carljm on `crates/red_knot/src/semantic.rs`:260 on 2024-06-05 20:55_

The walrus expression should record the symbol as defined, not as used; a use is a load of a name, not a store to it.

But we already do record it as defined! Because we visit `node.target` on the walrus, which will be a `Name` node with ctx `Store`, so we will recursively hit this `Name` case, with `current_definition` set to the walrus expression.

---

_@carljm reviewed on 2024-06-05 20:56_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:182 on 2024-06-05 20:56_

Just realized this comment is wrong/unnecessary, because unpacking assignment can't actually be used in a walrus expression.

---

_@AlexWaygood reviewed on 2024-06-05 20:57_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic.rs`:260 on 2024-06-05 20:57_

> The walrus expression should record the symbol as defined, not as used; a use is a load of a name, not a store to it.

Right, but in e.g. `x = (y := 1) + 2`, you could argue that `y` is being defined (assigned to `1`) and then _immediately_ used (added to `2`) in the same breath?

> But we already do record it as defined! Because we visit `node.target` on the walrus, which will be a `Name` node with ctx `Store`, so we will recursively hit this `Name` case, with `current_definition` set to the walrus expression.

Ah gotcha, thanks!

---

_@AlexWaygood reviewed on 2024-06-05 21:06_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic.rs`:260 on 2024-06-05 21:06_

Thinking of the walrus-created variable as unused is consistent with our current behaviour, though: it appears we flag `x` in the following snippet as unused in e.g. our `F841` lint rule:

```py
def foo():
    return (x := 1) + 2
```

And that sorta makes sense, since you can just write that more simply without the walrus as

```py
def foo():
    return 1 + 2
```

So I think I retract my argument. It only makes sense to think of walrus-defined variables as being "used" if they are _additionally_ used _outside_ of the immediate expression in which they are defined.

TL;DR: LGTM!

---

_@carljm reviewed on 2024-06-05 21:10_

---

_Review comment by @carljm on `crates/red_knot/src/semantic.rs`:260 on 2024-06-05 21:10_

> Right, but in e.g. x = (y := 1) + 2, you could argue that y is being defined (assigned to 1) and then immediately used (added to 2) in the same breath?

Aha! Yes, that's an interesting point. You could look at it that way, but this is not really the way Python looks at it, in terms of the AST (there is no `Name` node for `y` with context `Load` here) or in terms of the compiled bytecode (there is no `LOAD_FAST/LOAD_NAME` of `y` here).

For basically exactly the same reasons, it is not the simplest way for us to look at it, either. There is no "use" of `y` here because there is no need for us to look up `y` and resolve reachable definition(s) for it: we know that definition would always resolve to this very walrus expression, and when doing type inference we always already have the type of that expression in hand! So it's much simpler to look at it as simply "the type of the walrus expression is the type of its right-hand side, and as a side effect it also defines a name." This is how CPython handles it (except in terms of value, not type, of course), and how we handle it in this PR, and it doesn't require treating the walrus expression as a "use" of its target name. You could think of it instead as we are only "using" the right-hand-side expression, and also storing it off to a name as a side effect, but not "using" the name here.

---

_Merged by @carljm on 2024-06-05 21:13_

---

_Closed by @carljm on 2024-06-05 21:13_

---

_Branch deleted on 2024-06-05 21:13_

---
