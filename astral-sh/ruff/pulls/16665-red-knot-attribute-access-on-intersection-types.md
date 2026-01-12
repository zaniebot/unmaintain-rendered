```yaml
number: 16665
title: "[red-knot] Attribute access on intersection types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/attribute-access-on-intersections
created_at: 2025-03-12T09:29:38Z
updated_at: 2025-03-12T12:25:34Z
url: https://github.com/astral-sh/ruff/pull/16665
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Attribute access on intersection types

---

_@sharkdp_

## Summary

Implements attribute access on intersection types, which didn't previously work. For example:

```py
from typing import Any

class P: ...
class Q: ...

class A:
    x: P = P()

class B:
    x: Any = Q()

def _(obj: A):
    if isinstance(obj, B):
        reveal_type(obj.x)  # revealed: P & Any
```

Refers to [this comment].

[this comment]: https://github.com/astral-sh/ruff/pull/16416#discussion_r1985040363

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2025-03-12 09:29_

---

_Review requested from @carljm by @sharkdp on 2025-03-12 09:29_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-12 09:29_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-12 09:29_

---

_@AlexWaygood reviewed on 2025-03-12 09:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1061 on 2025-03-12 09:34_

I wonder if we should allow `type[Intersection[A, B]]` in type expressions, as a shorthand for `Intersection[type[A], type[B]]`. We already allow (and it's mandated by the spec) `type[A | B]` and `type[Union[A, B]]` as shorthands for `type[A] | type[B]`.

Obviously doesn't need to be done in this PR, though

---

_Comment by @github-actions[bot] on 2025-03-12 09:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
arrow (https://github.com/arrow-py/arrow)
- warning: lint:possibly-unbound-attribute
-    --> /tmp/mypy_primer/projects/arrow/arrow/factory.py:237:47
-     |
- 235 |             # (Arrow) -> from the object's datetime @ tzinfo
- 236 |             elif isinstance(arg, Arrow):
- 237 |                 return self.type.fromdatetime(arg.datetime, tzinfo=tz)
-     |                                               ------------ Attribute `datetime` on type `@Todo & Arrow & ~Decimal | float & Arrow` is possibly unbound
- 238 |
- 239 |             # (datetime) -> from datetime @ tzinfo
-     |
- 
- 
- error: lint:unresolved-attribute
-     |
-     |
- 
- error: lint:unresolved-attribute
-    --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:166:17
-     |
- 164 |             and hasattr(tzinfo, "localize")
- 165 |             and hasattr(tzinfo, "zone")
- 166 |             and tzinfo.zone
-     |                 ^^^^^^^^^^^ Type `@Todo & tzinfo` has no attribute `zone`
- 167 |         ):
- 168 |             tzinfo = parser.TzinfoParser.parse(tzinfo.zone)
-     |
-    --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:168:48
- 166 |             and tzinfo.zone
- 167 |         ):
- 168 |             tzinfo = parser.TzinfoParser.parse(tzinfo.zone)
-     |                                                ^^^^^^^^^^^ Type `@Todo & tzinfo` has no attribute `zone`
- 169 |         elif isinstance(tzinfo, str):
- 170 |             tzinfo = parser.TzinfoParser.parse(tzinfo)
- Found 80 diagnostics
+ Found 77 diagnostics

```
</details>


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1080 on 2025-03-12 10:37_

Again, not something for this PR necessarily. But I think that this implies that we should infer the following case like so:

```py
@final
class A: ...
class B: ...
class C:
    x: A
class D:
    x: B

def f(y: C):
    if isinstance(y, D):
        reveal_type(y)  # revealed: Never
```

Because we know that all instances of `C` have an `x` attribute, but the inferred type of the `x` attribute on the `y` variable would be `A & B`, which simplifies  to `Never`, since `A` is `@final`. An inferred attribute type of `Never` for the `x` attribute is the same thing as saying that `y` cannot have an attribute `x` in this branch,  but we already established that `y` _must_ have an `x` attribute, since `y` is an instance of `C`. The only way to solve this contradiction is to say that this branch is in fact unreachable: `C & D` must simplify to `Never` due to the fact that `A & B` simplifies to `Never`.

This is perhaps quite a long-winded proof for something that is self-evident...! I think mypy and pyright might also infer unreachable code in cases like this

---

_@AlexWaygood reviewed on 2025-03-12 10:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1080 on 2025-03-12 11:49_

We already (with this PR) correctly infer the type of `y.x` as `Never` in your example.

I think it is also correct to go from that to inferring `C & D` itself as `Never`, but this could be quite expensive to implement (since it would require checking every attribute of any pair of types that end up in an intersection together), and it's not clear to me how necessary/valuable it will be in practice.

---

_@carljm approved on 2025-03-12 11:59_

---

_Comment by @carljm on 2025-03-12 12:00_

(mypy-primer changes look correct)

---

_Merged by @sharkdp on 2025-03-12 12:20_

---

_Closed by @sharkdp on 2025-03-12 12:20_

---

_Branch deleted on 2025-03-12 12:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1080 on 2025-03-12 12:25_

> We already (with this PR) correctly infer the type of `y.x` as `Never` in your example.

I know; it was the further inference that I was attempting to discuss.

Anyway, I agree that we can defer this for now and see how expensive and/or useful it is later!

---

_@AlexWaygood reviewed on 2025-03-12 12:25_

---
