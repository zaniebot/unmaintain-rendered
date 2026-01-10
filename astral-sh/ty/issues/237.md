```yaml
number: 237
title: Implement Homogeneous Tuple Type
type: issue
state: closed
author: cake-monotone
labels:
  - generics
assignees: []
created_at: 2024-10-21T11:02:55Z
updated_at: 2025-05-13T02:02:26Z
url: https://github.com/astral-sh/ty/issues/237
synced_at: 2026-01-10T02:34:09Z
```

# Implement Homogeneous Tuple Type

---

_Issue opened by @cake-monotone on 2024-10-21 11:02_

As `Type::Tuple` is becoming more actively used in implementations, I think we should implement Homogeneous Tuples before the refactoring cost becomes too high.

I’m considering an implementation like the following using an enum:

<details>
<summary>Implementation blueprint (Outdated) </summary>

```rs
#[derive(Copy, Clone, Debug, PartialEq, Eq, Hash)]
pub enum TupleType<'db> {
    Heterogeneous(HeterogeneousTupleType<'db>),
    Homogeneous(HomogeneousTupleType<'db>),
}

#[salsa::interned]
pub struct HeterogeneousTupleType<'db> {
    elements: Box<[Type<'db>]>,
}

#[salsa::interned]
pub struct HomogeneousTupleType<'db> {
    element: Type<'db>,
}
```

</details>

## Expected Issues

Currently, it seems that there is no way to assign a homogeneous tuple type in the tests.

Do you have any suggestions for a proper way to handle this? Or should we wait for another feature to be merged first?

---

_Comment by @AlexWaygood on 2024-10-21 11:22_

Thanks for opening this issue!

I think homogenous tuples will require much less special-casing from us than heterogenous tuples (thankfully!). Whereas heterogenous tuples need to be extensively special-cased in red-knot, homogenous tuples are pretty similar to most other generic sequence types -- I think we should simply be able to fallback to [typeshed's annotations for `tuple`](https://github.com/astral-sh/ruff/blob/f3612c271714c18ab9a4c2d3976b12c85bb37653/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi#L949-L973) for most simple operations.

We don't yet have any representation of generic types in red-knot at all, however. This is a large outstanding task, and something that I know @carljm plans to work on soon. I think we should probably hold off on implementing anything to do with homogenous tuples until we have an initial implementation of generic types.

We will of course have to have _some_ special handling of homogenous tuples in due course:
- We need to understand where heterogenous tuples are and aren't subtypes of homogenous tuples
- We'll need to understand somewhat terrifying types such as `tuple[int, *tuple[str, ...], bytes]` which combine heterogenous and homogenous tuples.

But I still expect it to be much more limited than the special casing we've already implemented for heterogenous tuples. And it will possibly be implemented in a completely different way.

---

_Comment by @cake-monotone on 2024-10-21 11:34_

Thank you for the detailed answer! I’m leaving a few special cases I had in mind for future consideration:

 -  `tuple[*tuple[str, ...], ...]` is invalid.
 -  `tuple[*tuple[int, ...]]` should be inferred as `tuple[int, ...]`.

---

_Comment by @AlexWaygood on 2024-10-21 11:36_

We'll also need to make sure we understand that `tuple[str, *tuple[int, ...]]` and `tuple[str, typing.Unpack[tuple[int, ...]]]` are the same type.

---

_Comment by @MichaReiser on 2024-10-21 12:44_

Nit:

```diff
 #[salsa::interned]
 pub struct HomogeneousTupleType<'db> {
-     element: Box<Type<'db>>,
+     element: Type<'db>,
 }

---

_Comment by @carljm on 2024-10-21 18:03_

It may be that the existing `Type::Tuple` should be renamed to make it more clear that it represents only heterogeneous tuple types, but I'm hesitant to spend too much time on renamings until we see the full shape of our tuple support.

---

_Comment by @carljm on 2024-11-07 16:54_

depends on https://github.com/astral-sh/ty/issues/230

---

_Renamed from "[red-knot] Implement Homogeneous Tuple Type" to "Implement Homogeneous Tuple Type" by @MichaReiser on 2025-05-07 15:27_

---

_Label `generics` added by @AlexWaygood on 2025-05-10 20:01_

---

_Closed by @AlexWaygood on 2025-05-13 02:02_

---
