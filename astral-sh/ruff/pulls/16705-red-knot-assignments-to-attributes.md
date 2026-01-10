```yaml
number: 16705
title: "[red-knot] Assignments to attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/attribute-write-access
created_at: 2025-03-13T12:26:05Z
updated_at: 2025-03-14T23:47:13Z
url: https://github.com/astral-sh/ruff/pull/16705
synced_at: 2026-01-10T19:49:02Z
```

# [red-knot] Assignments to attributes

---

_Pull request opened by @sharkdp on 2025-03-13 12:26_

## Summary

This changeset adds proper support for assignments to attributes:
```py
obj.attr = value
```

In particular, the following new features are now available:

* We previously didn't raise any errors if you tried to assign to a non-existing attribute `attr`. This is now fixed.
* If `type(obj).attr` is a data descriptor, we now call its `__set__` method instead of trying to assign to the load-context type of `obj.attr`, which can be different for data descriptors.
* An initial attempt was made to support unions and intersections, as well as possibly-unbound situations. There are some remaining TODOs in tests, but they only affect edge cases. Having nested diagnostics would be one way that could help solve the remaining cases, I believe.

## Follow ups

The following things are planned as follow-ups:

- Write a test suite with snapshot diagnostics for various attribute assignment errors
- Improve the diagnostics. An easy improvement would be to highlight the right hand side of the assignment as a secondary span (with the rhs type as additional information). Some other ideas are mentioned in TODO comments in this PR.
- Improve the union/intersection/possible-unboundness handling
- Add support for calling custom `__setattr__` methods (see new false positive in the ecosystem results)

## Ecosystem changes

Some changes are related to assignments on attributes with a custom `__setattr__` method (see above). Since we didn't notice missing attributes at all in store context previously, these are new.

The other changes are related to properties. We previously used their read-context type to test the assignment. That results in weird error messages, as we often see assignments to `self.property` and then we think that those are instance attributes *and* descriptors, leading to union types. Now we properly look them up on the meta type, see the decorated function, and try to overwrite it with the new value (as we don't understand decorators yet). Long story short: the errors are still weird, we need to understand decorators to make them go away.

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2025-03-13 12:26_

---

_Comment by @github-actions[bot] on 2025-03-13 12:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
+    |
+ 
+ error: lint:unresolved-attribute
+   --> /tmp/mypy_primer/projects/zipp/zipp/compat/overlay.py:34:1
+    |
+ 33 | zipfile = HashableNamespace(**vars(importlib.import_module('zipfile')))
+ 34 | zipfile.Path = zipp.Path
+    | ^^^^^^^^^^^^ Unresolved attribute `Path` on type `HashableNamespace`.
+ 35 | zipfile._path = zipp
+    |
+ 
+ error: lint:unresolved-attribute
+   --> /tmp/mypy_primer/projects/zipp/zipp/compat/overlay.py:35:1
+    |
+ 33 | zipfile = HashableNamespace(**vars(importlib.import_module('zipfile')))
+ 34 | zipfile.Path = zipp.Path
+ 35 | zipfile._path = zipp
+    | ^^^^^^^^^^^^^ Unresolved attribute `_path` on type `HashableNamespace`.
+ 36 |
+ 37 | sys.modules[__name__ + '.zipfile'] = zipfile  # type: ignore[assignment]
- Found 8 diagnostics
+ Found 10 diagnostics

black (https://github.com/psf/black)
+ 
+     |
+     |
-     |             ^^^^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Leaf`>`
+     |             ^^^^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
-     |             ^^^^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Leaf`>`
+     |             ^^^^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
-     |                 ^^^^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Leaf`>`
+     |                 ^^^^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
+ error: lint:invalid-assignment
+     |         ^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
+     |         ^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
+    --> /tmp/mypy_primer/projects/black/src/black/ranges.py:330:5
+ 328 |     # this is actually required to not break incremental reformatting.
+ 329 |     prefix = first.prefix
+ 330 |     first.prefix = ""
-     |         ^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Leaf`>`
+     |     ^^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` on type `Leaf & ~AlwaysFalsy`
+ 331 |     index = node.remove()
+ 332 |     if index is not None:
-     |         ^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Leaf`>`
+    --> /tmp/mypy_primer/projects/black/src/black/ranges.py:356:5
+ 354 |         return
+ 355 |     prefix = first.prefix
+ 356 |     first.prefix = ""
+     |     ^^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` on type `Leaf & ~AlwaysFalsy`
+ 357 |     value = "".join(str(node) for node in nodes)
+ 358 |     # The prefix comment on the NEWLINE leaf is the trailing comment of the statement.
+     |         ^^^^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
-     |         ^^^^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Leaf`>`
-    --> /tmp/mypy_primer/projects/black/src/black/linegen.py:158:17
- 157 |             if any_open_brackets:
- 158 |                 node.prefix = ""
-     |                 ^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `@Todo & <bound method `prefix` of `Leaf`>`
- 159 |             if node.type not in WHITESPACE:
- 160 |                 self.current_line.append(node)
-     |             ^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Node`> | Literal[prefix]`
+     |             ^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
-     |             ^^^^^^^^^^^ Object of type `<bound method `prefix` of `Node`> | Literal[prefix]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Leaf`>`
+     |             ^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
-     |                 ^^^^^^^^^^^ Object of type `Literal[""]` is not assignable to attribute `prefix` of type `<bound method `prefix` of `Node`> | Literal[prefix]`
+     |                 ^^^^^^^^^^^ Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
- Found 298 diagnostics
+ Found 299 diagnostics

```
</details>


---

_Renamed from "[red-knot] Writes to attributes" to "[red-knot] Assignments to attributes" by @sharkdp on 2025-03-13 13:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2366 on 2025-03-13 20:04_

There is quite a bit of unfortunate code duplication with the branch above. On the other hand, there's also quite a bit of special handling (e.g. dedicated diagnostics for `ClassVar`s above, improved error messages for accidental class-level assignments to instance attributes here) in the dedicated branches that would make it cumbersome to generalize this into a single method that is being parametrized. Happy to attempt to do this though if we think it's worth it.

---

_@sharkdp reviewed on 2025-03-13 20:04_

---

_Marked ready for review by @sharkdp on 2025-03-13 20:09_

---

_Review requested from @carljm by @sharkdp on 2025-03-13 20:09_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-13 20:09_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-13 20:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:38 on 2025-03-13 22:59_

We will want this diagnostic (and others below) to name the expected type, but this is fine to leave as follow up for when we have the new diagnostics system.

I think "data descriptor attribute... with custom `__set__` method" is perhaps redundant? But also fine for now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1226 on 2025-03-13 23:00_

"possibly unbound" is a weird phrasing for an attribute we are assigning to, but we can revisit this in future.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:533 on 2025-03-13 23:10_

This still deserves a TODO like before -- the error has changed but its still wrong, pending property support.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2223 on 2025-03-13 23:18_

This is off the top of my head, but I think we can treat all negative intersection elements as `object` for this purpose (I don't think a negative type ever gives us information about attributes.) And because any positive element must inherit `object`, I think we actually can totally ignore negative elements except in the case where there are zero positive elements, in which case we treat the entire intersection as implicitly `object`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2366 on 2025-03-13 23:21_

This looks sufficiently different to me, both semantically and in the actual code, that it seems better to keep it separate.

---

_@carljm approved on 2025-03-13 23:28_

Looks great!

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:38 on 2025-03-14 10:18_

> We will want this diagnostic (and others below) to name the expected type

Yes, absolutely. I didn't attempt to do this here, as I think it would naturally fall out of a sub-diagnostic that would include the call binding error.

---

_@sharkdp reviewed on 2025-03-14 10:18_

---

_@sharkdp reviewed on 2025-03-14 10:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2223 on 2025-03-14 10:45_

> This is off the top of my head, but I think we can treat all negative intersection elements as `object` for this purpose (I don't think a negative type ever gives us information about attributes.)

Let's consider two extreme cases. `~object = object & ~object = Never` and `~Never = object & ~Never = object`. In the former case, *every* attribute would exist (with type `Never`). So every attribute assignment to `~object` should succeed. In the latter case, only the attributes on `object` exist, so almost every attribute assignment to `~Never` should fail.

So in this sense, negative intersection elements do seem to provide information about attributes? I think we have special handling for these cases and we would not end up in this branch, but I struggled to justify why we can completely neglect negative elements here.

---

_Merged by @sharkdp on 2025-03-14 11:15_

---

_Closed by @sharkdp on 2025-03-14 11:15_

---

_Branch deleted on 2025-03-14 11:15_

---

_@carljm reviewed on 2025-03-14 23:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2223 on 2025-03-14 23:47_

Yes, I was implicitly excluding from consideration `Not[object]`, since that will simplify to `Never`, and `Not[Never]` since that will simplify to `object` (if alone) or disappear from the intersection.

In the realm of non-total negative elements that can actually exist in our intersection representation, I don't think that negative elements can contribute information about attributes, because the type system is not closed; there are always infinite possible objects with infinite possible attributes inhabiting a negation type.

---
