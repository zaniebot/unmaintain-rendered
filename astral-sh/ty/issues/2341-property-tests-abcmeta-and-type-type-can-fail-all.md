---
number: 2341
title: "(property tests) ABCMeta and `type[type]` can fail `all_fully_static_type_pairs_are_subtype_of_their_union` invariant"
type: issue
state: open
author: github-actions
labels:
  - bug
  - type properties
assignees: []
created_at: 2026-01-05T12:12:50Z
updated_at: 2026-01-06T00:58:57Z
url: https://github.com/astral-sh/ty/issues/2341
synced_at: 2026-01-10T01:51:14Z
---

# (property tests) ABCMeta and `type[type]` can fail `all_fully_static_type_pairs_are_subtype_of_their_union` invariant

---

_Issue opened by @github-actions on 2026-01-05 12:12_

Run listed here: https://github.com/astral-sh/ty/actions/runs/20714754042

---

_Label `bug` added by @github-actions[bot] on 2026-01-05 12:12_

---

_Label `type properties` added by @github-actions[bot] on 2026-01-05 12:12_

---

_Comment by @AlexWaygood on 2026-01-05 12:14_

```
failures:

---- types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union stdout ----

<-- snip -->

Failing types were:
type[type]
<class 'ABCMeta'> | ((type[type], /) -> type)

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' (4504) panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (FullyStaticTy(SubclassOfBuiltinClass("type")), FullyStaticTy(Union([AbcClassLiteral("ABCMeta"), Callable { params: List([Param { kind: PositionalOnly, name: None, annotated_ty: Some(SubclassOfBuiltinClass("type")), default_ty: None }]), returns: Some(SubclassOfBuiltinClass("object")) }])))
```

---

_Comment by @AlexWaygood on 2026-01-05 12:41_

Here's a more minimal reproduction of the bug:

https://play.ty.dev/a9c72bda-7649-4765-8615-c861d56e855b

```py
from abc import ABCMeta
from ty_extensions import is_subtype_of
from typing import reveal_type, Callable

# revealed: `Literal[False]`, but should be `Literal[True]`
reveal_type(bool(is_subtype_of(type[ABCMeta], type[type] | type[ABCMeta] | Callable[[type], type])))
```

The condition is understood as evaluating to `True` (as expected) if either the first or second element of the union on the right-hand side is removed.

Weirdly, _this_ (swapping out `ABCMeta` for a custom metaclass) does not seem to reproduce the bug:

```py
from ty_extensions import is_subtype_of
from typing import reveal_type, Callable

class Foo(type): ...

reveal_type(bool(is_subtype_of(type[Foo], type[type] | type[Foo] | Callable[[type], type])))
```

---

_Comment by @carljm on 2026-01-06 00:56_

Interesting -- this doesn't seem to be a regression? At least I can't find a version on which your minimal repro passes. Rather it seems to be a case that the property tests are just very unlikely to generate by accident, and we finally happened to hit it.

---

_Comment by @carljm on 2026-01-06 00:57_

Given that, I'm going to rename the issue and treat it like any other bug, rather than treating it as something we need to fix immediately in order to keep the property tests usable.

---

_Renamed from "Daily property test run failed on Mon Jan 05 2026" to "(property tests) ABCMeta and `type[type]` can fail `all_fully_static_type_pairs_are_subtype_of_their_union` invariant" by @carljm on 2026-01-06 00:58_

---
