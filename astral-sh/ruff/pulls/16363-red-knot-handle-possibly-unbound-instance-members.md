```yaml
number: 16363
title: "[red-knot] Handle possibly-unbound instance members"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/possibly-unbound-instance-members
created_at: 2025-02-25T10:01:20Z
updated_at: 2025-02-25T21:07:14Z
url: https://github.com/astral-sh/ruff/pull/16363
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Handle possibly-unbound instance members

---

_Pull request opened by @sharkdp on 2025-02-25 10:01_

## Summary

Adds support for possibly-unbound/undeclared instance members.

## Test Plan

New MD tests.

---

_Label `red-knot` added by @sharkdp on 2025-02-25 10:01_

---

_Review requested from @carljm by @sharkdp on 2025-02-25 10:01_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-25 10:01_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-25 10:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:899 on 2025-02-25 10:03_

We came to the same conclusion in an earlier PR. This is also mentioned higher up in this test suite. And mypy/pyright don't emit a diagnostic for this either (which is also mentioned higher up). This test is only added here for easier reference / completeness.

---

_@sharkdp reviewed on 2025-02-25 10:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:816 on 2025-02-25 14:17_

I guess for this one, I'm not totally clear on how it's different to https://github.com/astral-sh/ruff/pull/16363/files#r1969441733. All members of the union have an `x` attribute here, and like https://github.com/astral-sh/ruff/pull/16363/files#r1969441733, it's still an instance attribute that we can't verify has been definitely defined. Like https://github.com/astral-sh/ruff/pull/16363/files#r1969441733, there's still the issue that it's (unfortunately) not uncommon in Python to initialize attributes on instances after the instance has been constructed.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:407 on 2025-02-25 14:23_

is it worth generalising this optimisation inside the `UnionBuilder` rather than special-casing single-element unions here? The `UnionBuilder::elements` field is currently just `Vec<Type>`, but if it was something like `SmallVec<[Type; 1]>`, that would avoid allocating inside the union builder if there was only 1 element

---

_@AlexWaygood approved on 2025-02-25 14:24_

Looks good! A couple of minor questions

---

_@MichaReiser reviewed on 2025-02-25 14:32_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:407 on 2025-02-25 14:32_

I'd probably prefer the approach that I started in https://github.com/astral-sh/ruff/pull/16218 over using a `SmallVec`. Each access in a small vec has an added cost because it first need to check if it's a stack or heap allocated vec. Although using `from_elements` might be challenging here. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:407 on 2025-02-25 14:43_

> Although using `from_elements` might be challenging here.

yeah, I think it's probably not practical to use it here unfortunately

---

_@AlexWaygood reviewed on 2025-02-25 14:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:407 on 2025-02-25 15:27_

Here and below `TypeQualifiers::empty()` doesn't feel right, but maybe it doesn't make sense to address this until we have support for more type qualifiers? We are throwing away type qualifiers from the `match` above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:427 on 2025-02-25 15:29_

Here also we are throwing away type-qualifier information from whatever superclass attribute(s) we found.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:816 on 2025-02-25 15:34_

The distinction this PR appears to be making is that we'll report boundness from the class body accurately, but when we find implicit attributes assigned in methods, we'll presume "always bound" for those. I'm not 100% sure where we'll end up here in practice, but this seems like a reasonable place to start: making something conditional in a class body is unusual, and to do that with a non-statically-known branch condition even more so.

That said, I do think there's a not-insignificant chance that in the face of feedback from real codebases, we end up rolling back most of our possibly-unbound diagnostics on methods and attributes, or making them opt-in.

---

_@carljm approved on 2025-02-25 15:34_

---

_Comment by @carljm on 2025-02-25 15:37_

The handling of inherited attributes here reminds me that we will also need to add eager Liskov checking for inherited classes (once we have callable types and can do callable subtype checks). For a mutable attribute (which all are by default) a strict Liskov interpretation would require invariance, which would make the union handling here irrelevant.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:816 on 2025-02-25 15:39_

That's a good question. I modeled it after the behavior on the class object, but we may want to design that differently. Should possible unboundness always be ignored for instance members?

---

_@sharkdp reviewed on 2025-02-25 15:39_

---

_@sharkdp reviewed on 2025-02-25 15:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:816 on 2025-02-25 15:42_

Oops. Didn't see @carljm's answer. Does that mean that we can keep the current behavior for now?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:816 on 2025-02-25 15:48_

> Should possible unboundness always be ignored for instance members?

I would argue yes, but I'm also fine with deferring this for now. Could possibly add a TODO saying that this could be revisited in the future?

---

_@AlexWaygood reviewed on 2025-02-25 15:48_

---

_Comment by @sharkdp on 2025-02-25 15:59_

> which would make the union handling here irrelevant

Mainly curious: is this true even in the presence of gradual types? Could a subclass refine a base-class `Any` attribute to a more static type `T`? And would it then make sense to treat a possibly-unbound-on-child-class attribute as being of type `Any | T`?

---

_Comment by @carljm on 2025-02-25 16:18_

> Mainly curious: is this true even in the presence of gradual types? Could a subclass refine a base-class `Any` attribute to a more static type `T`? And would it then make sense to treat a possibly-unbound-on-child-class attribute as being of type `Any | T`?

That's a good point: I think that would make sense.

---

_@sharkdp reviewed on 2025-02-25 18:26_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:407 on 2025-02-25 18:26_

Yes, thanks. Added a test case, preliminary handling of type qualifiers (without diagnostics for conflicting qualifiers), and a TODO.

---

_@sharkdp reviewed on 2025-02-25 18:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:816 on 2025-02-25 18:27_

Done.

---

_@AlexWaygood reviewed on 2025-02-25 18:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:813 on 2025-02-25 18:33_

```suggestion
    # Note: we might want to consider ignoring possibly-unbound diagnostics for instance attributes eventually,
```

---

_@sharkdp reviewed on 2025-02-25 18:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:813 on 2025-02-25 18:39_

I … what? :smile: Thanks!

---

_@AlexWaygood reviewed on 2025-02-25 18:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:813 on 2025-02-25 18:40_

hehe, no worries

---

_Merged by @sharkdp on 2025-02-25 19:00_

---

_Closed by @sharkdp on 2025-02-25 19:00_

---

_Branch deleted on 2025-02-25 19:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:400 on 2025-02-25 21:06_

Probably the diagnostic would be raised by an eager Liskov check at the inheritance site, rather than here? But it doesn't hurt to have a TODO here until we do that.

---

_@carljm reviewed on 2025-02-25 21:07_

---
