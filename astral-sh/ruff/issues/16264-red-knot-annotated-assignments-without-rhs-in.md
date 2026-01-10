```yaml
number: 16264
title: "[red-knot] annotated assignments without RHS in stubs should still be bindings"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-02-19T20:30:47Z
updated_at: 2025-02-28T16:45:23Z
url: https://github.com/astral-sh/ruff/issues/16264
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] annotated assignments without RHS in stubs should still be bindings

---

_Issue opened by @carljm on 2025-02-19 20:30_

### Description

Otherwise, we get into trouble when a special form (say `Protocol`) is used within the global scope of the same stub file where it is defined, because that's just a local use, which only respects bindings.

This doesn't affect special forms that are used in annotation position (e.g. `ClassVar`) because all annotation expressions are lazy in stub files, meaning they use the public type.

---

_Label `red-knot` added by @carljm on 2025-02-19 20:30_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-02-19 20:30_

---

_Comment by @carljm on 2025-02-21 19:01_

We probably also want to do this generally in stubs, including in class body scopes, because people often omit right-hand-sides from annotated assignments in stubs, so we can't assume that `x: int` in a class body in a stub is an instance-only attribute.

Perhaps in an ideal world, the recommended approach in stubs would be to include class-level defaults (or at least `...` as RHS) so that type checkers can distinguish pure-instance variables from instance-or-class variables. But since this isn't the current practice, we probably need to adapt.

This cropped up as causing a false positive in tomllib, see https://github.com/astral-sh/ruff/pull/16036#discussion_r1966011718

---

_Comment by @mishamsk on 2025-02-21 19:37_

btw. in one of the early versions of #16036 I actually had logic that special cased stubs & ABC's, but @sharkdp asked to remove it here https://github.com/astral-sh/ruff/pull/16036#discussion_r1950851155 - I have the code stashed, including the ABC wiring - so again, just saying, I'd be happy to re-wire the boundness/declaredness.

---

_Comment by @carljm on 2025-02-21 20:23_

I think @sharkdp was right that we should handle `ClassVar` specially (which the PR now does). And I think we will probably want to implement the special case for stubs at a deeper layer, since it should impact both our interpretation of class vs instance attributes in class bodies, and also our interpretation of RHS-less annotated assignments at module level (as discussed in the OP here.)

By "a deeper layer" I mean that already when we create the initial Definitions in building the semantic index, we should always consider an annotated assignment in a stub with no RHS to be both a binding and a declaration, rather than just a declaration. And then in type inference we should record it as both a binding and a declaration of the annotated type. This reflects the fact that in general, in a stub file, `x: int` is used to imply `x: int = ...` where `...` is something of type `int`.

I think implementing this generalized approach will also help with resolution of the symbol boundness-vs-declaredness issue. There's a TODO comment in `symbol_by_id` that says this needs design work, and stubs might need to behave differently, but I think this issue captures the precise way in which stubs should behave differently -- if we special-case stubs in Definition creation in the way described above, we shouldn't need to special-case them in how we resolve symbols.

---

_Comment by @mishamsk on 2025-02-21 21:24_

> By "a deeper layer" I mean that already when we create the initial Definitions in building the semantic index

Oh, I'd love to implement this way. I haven't touched semantic index so far, so will be a great opportunity to take my relationship  with it to the next level!

> There's a TODO comment

Yeah, I've seen that from the very beginning ;-) 

---

_Comment by @mishamsk on 2025-02-24 17:37_

Now that #16036 is merged, shall I take a stab at this one per @carljm suggested approach?

---

_Comment by @carljm on 2025-02-24 17:38_

> Now that [#16036](https://github.com/astral-sh/ruff/pull/16036) is merged, shall I take a stab at this one per [@carljm](https://github.com/carljm) suggested approach?

That would be great!

---

_Comment by @mishamsk on 2025-02-26 03:05_

@carljm just FYI, as this is not formally assigned to me - I started working on this, but not ready for review yet. I can push a draft PR so it will be visible here, if this helps. Or just wait until it is ready for review

---

_Assigned to @mishamsk by @carljm on 2025-02-26 03:25_

---

_Comment by @carljm on 2025-02-26 03:26_

Now it's official! Feel free to push a draft PR whenever you want CI feedback. 

---

_Closed by @AlexWaygood on 2025-02-28 16:45_

---
