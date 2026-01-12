```yaml
number: 16269
title: "[red-knot] MDTest: Use custom class names instead of builtins"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/opaque-type-names
created_at: 2025-02-20T11:25:23Z
updated_at: 2025-02-20T12:25:56Z
url: https://github.com/astral-sh/ruff/pull/16269
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] MDTest: Use custom class names instead of builtins

---

_@sharkdp_

## Summary

Follow up on the discussion [here](https://github.com/astral-sh/ruff/pull/16121#discussion_r1962973298). Replace builtin classes with custom placeholder names, which should hopefully make the tests a bit easier to understand.

I carefully renamed things one after the other, to make sure that there is no functional change in the tests.

---

_Label `testing` added by @sharkdp on 2025-02-20 11:25_

---

_Label `red-knot` added by @sharkdp on 2025-02-20 11:25_

---

_Review requested from @carljm by @sharkdp on 2025-02-20 11:25_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-20 11:25_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-20 11:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:197 on 2025-02-20 11:27_

should we also be returning something custom here, rather than `set`?

---

_@sharkdp reviewed on 2025-02-20 11:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:249 on 2025-02-20 11:28_

We could get rid of all of these `could_raise_returns_X` functions and just call `X()` directly (which could also raise). This would decrease the verbosity a lot. On the other hand, it makes the "could raise" aspect in the tests a little less clear. I'm okay with both variants.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:454 on 2025-02-20 11:29_

you can tell that I was kinda running out of builtin classes to use in the test at this point :P

---

_@AlexWaygood approved on 2025-02-20 11:29_

---

_@AlexWaygood reviewed on 2025-02-20 11:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:249 on 2025-02-20 11:30_

Yeah, I do agree that the test setup is unfortunately somewhat verbose. But I kinda prefer them how they are, for this reason:

> On the other hand, it makes the "could raise" aspect in the tests a little less clear.

---

_@sharkdp reviewed on 2025-02-20 11:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:454 on 2025-02-20 11:31_

Yes, haha :smile: 

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:153 on 2025-02-20 11:46_

@AlexWaygood I found a few more instances here. Would you mind having a look at this particular test. I'm not quite sure what to do with the `other: str` annotations in `class B` above. Should those be changed to `LtReturnType`, or is `str` just *some unrelated type* here?

---

_@sharkdp reviewed on 2025-02-20 11:46_

---

_@AlexWaygood reviewed on 2025-02-20 12:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:153 on 2025-02-20 12:02_

> Should those be changed to `LtReturnType`, or is `str` just _some unrelated type_ here?

The latter -- `str` here is just "something that is not `object`, `A` or `B`". I think the best thing would be to create a new `class Unrelated: ...` definition and use that in the annotations for the `other` parameter in `B.__eq__` and `B.__ne__`.

---

_Merged by @sharkdp on 2025-02-20 12:25_

---

_Closed by @sharkdp on 2025-02-20 12:25_

---

_Branch deleted on 2025-02-20 12:25_

---
