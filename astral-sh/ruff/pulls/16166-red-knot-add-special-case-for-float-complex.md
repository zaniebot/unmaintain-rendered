```yaml
number: 16166
title: "[red-knot] add special case for float/complex"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/intfloat
created_at: 2025-02-14T16:58:57Z
updated_at: 2025-02-14T20:24:13Z
url: https://github.com/astral-sh/ruff/pull/16166
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] add special case for float/complex

---

_Pull request opened by @carljm on 2025-02-14 16:58_

When adjusting the existing tests, I aimed to avoid dealing with the special case in other tests if it's not necessary to do so (that is, avoid using `float` and `complex` as examples where we just need "some type"), and keep the tests for the special case mostly collected in the mdtest dedicated to that purpose.

Fixes https://github.com/astral-sh/ruff/issues/14932


---

_Label `red-knot` added by @carljm on 2025-02-14 16:58_

---

_@sharkdp reviewed on 2025-02-14 17:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:33 on 2025-02-14 17:39_

Not sure if we have to, but we could potentially keep those tests using something like
```py
type BareFloat = knot_extensions.TypeOf[1.0]
```

---

_@carljm reviewed on 2025-02-14 19:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:33 on 2025-02-14 19:09_

Done!

---

_Marked ready for review by @carljm on 2025-02-14 19:18_

---

_Review requested from @MichaReiser by @carljm on 2025-02-14 19:18_

---

_Review requested from @AlexWaygood by @carljm on 2025-02-14 19:18_

---

_@MichaReiser reviewed on 2025-02-14 19:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:274 on 2025-02-14 19:19_

Should we create and link to an issue?

---

_@MichaReiser approved on 2025-02-14 19:20_

I didn't review the semantic changes but that's an easy logic change indeed (the test fall outs are the more annoying part)

---

_@InSyncWithFoo reviewed on 2025-02-14 19:22_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/int_float_complex.md`:48 on 2025-02-14 19:22_

How would the hover popup for `x: float` look like in the context of an IDE?

Something like this?

> ```python
> x: int | float
> ```


---

_@carljm reviewed on 2025-02-14 19:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:274 on 2025-02-14 19:22_

I'm not confident enough that this is worth doing or necessary to justify an issue, or even a TODO; I felt like a mention in the mdtest was the right level of preserving some context for future discussion, in case it comes up. (In other words, I don't feel it's necessary to track this as something we need to remember to come back to, it's more an "if it comes up in the real world" situation.)

---

_@carljm reviewed on 2025-02-14 19:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/int_float_complex.md`:48 on 2025-02-14 19:24_

Yes, something like that. My feeling is that this special case is confusing no matter what, but even more confusing if you try to hide it, so I would prefer that we just show the real type.

---

_@carljm reviewed on 2025-02-14 19:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:33 on 2025-02-14 19:26_

(Also, it's really cool that we can do this!)

---

_@InSyncWithFoo reviewed on 2025-02-14 19:33_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/int_float_complex.md`:48 on 2025-02-14 19:33_

It should perhaps say:

> ```python
> x: int | float
> ```
>
> Note: `float` was promoted to `int | float` as a special case.

(...with a better explanation, of course.)

For reference, here's a Kotlin hover popup by IntelliJ:

![](https://github.com/user-attachments/assets/9d4de188-7c39-478b-8c70-0c5785e940c8)

---

_@carljm reviewed on 2025-02-14 20:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/int_float_complex.md`:48 on 2025-02-14 20:24_

Yes, we could consider adding such a feature.

---

_Merged by @carljm on 2025-02-14 20:24_

---

_Closed by @carljm on 2025-02-14 20:24_

---

_Branch deleted on 2025-02-14 20:24_

---
