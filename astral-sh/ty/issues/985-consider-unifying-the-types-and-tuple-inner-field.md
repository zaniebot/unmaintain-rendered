```yaml
number: 985
title: "Consider unifying the `types` and `tuple_inner` field in `Specialization`"
type: issue
state: open
author: AlexWaygood
labels:
  - performance
  - generics
  - memory
assignees: []
created_at: 2025-08-14T11:11:32Z
updated_at: 2025-08-14T13:10:24Z
url: https://github.com/astral-sh/ty/issues/985
synced_at: 2026-01-12T15:54:24Z
```

# Consider unifying the `types` and `tuple_inner` field in `Specialization`

---

_@AlexWaygood_

Currently the `Specialization` struct in `tuple.rs` looks like this:

```rs
#[salsa::interned(debug, heap_size=ruff_memory_usage::heap_size)]
pub struct Specialization<'db> {
    pub(crate) generic_context: GenericContext<'db>,
    #[returns(deref)]
    pub(crate) types: Box<[Type<'db>]>,

    /// For specializations of `tuple`, we also store more detailed information about the tuple's
    /// elements, above what the class's (single) typevar can represent.
    tuple_inner: Option<TupleType<'db>>,
}
```

Conceptually, however:
- A class either is a tuple or it isn't!
- If a class is _not_ a tuple, `tuple_inner` will always be `None`
- If a class _is_ a tuple, `tuple_inner` should always be `Some()`, `types` should always be length 1, and the single `Type` in the `types` boxed slice should be the union of all the elements of the tuple in `tuple_inner`

It feels like we have some redundancy here, therefore; a design that uses less memory (and which might be more elegant) might look like this:

```rs
#[derive(Debug, Clone, PartialEq, Eq, Hash, getsize_2::GetSize)]
enum SpecializationInner<'db> {
    Tuple(TupleType<'db>),
    NonTuple(Box<[Type<'db>]>),
}

#[salsa::interned(debug, heap_size=ruff_memory_usage::heap_size)]
pub struct Specialization<'db> {
    pub(crate) generic_context: GenericContext<'db>,
    #[returns(ref)]
    inner: SpecializationInner<'db>
}

impl<'db> SpecializationInner<'db> {
    pub(crate) fn types(self, &'db dyn Db) -> &[Type<'db>] {
        #[salsa::tracked(returns(ref))]
        fn homogeneous_element_type<'db>(db: &'db dyn Db, tuple: TupleType<'db>) -> Type<'db> {
            tuple.tuple(db).homogeneous_element_type(db)
        }

        match self.inner(db) {
            SpecializationInner::Tuple(tuple) => std::slice::from_ref(homogeneous_element_type(*tuple)),
            SpecializationInner::NonTuple(types) => types,
        }
    }
}
```

I've briefly tried to make this change but... I don't think I'm the right person to do this refactor; I just don't understand our generics internals well enough right now :-)

---

_Label `performance` added by @AlexWaygood on 2025-08-14 11:11_

---

_Label `generics` added by @AlexWaygood on 2025-08-14 11:11_

---

_Label `memory` added by @AlexWaygood on 2025-08-14 11:11_

---

_Comment by @MichaReiser on 2025-08-14 11:14_

Nit: We could also keep the storage as (minus `tuple_inner`) and assume it's a tuple if and only if:

* types has length 1
* the only element is a tuple



---

_Comment by @AlexWaygood on 2025-08-14 11:21_

> Nit: We could also keep the storage as (minus `tuple_inner`) and assume it's a tuple if and only if:
> 
>     * types has length 1
> 
>     * the only element is a tuple

Hmm, I don't think that would work. If the `Specialization` represents the specialization of the tuple type `tuple[int, str]`, for example, then on `main` the `types` field would be `[int | str]` (a length-one slice where the single element is not a tuple, but it is the union of the two tuple elements), and `tuple_inner` would be `Some(tuple[int, str])`. If the `types` field has length one and the single type is a tuple, then applying that specialization to a tuple type would imply something like `tuple[tuple[int, str], ...]`

---

_Comment by @dcreager on 2025-08-14 13:10_

> I've briefly tried to make this change but... I don't think I'm the right person to do this refactor; I just don't understand our generics internals well enough right now

Your code snippets look like it should™ be most of what's needed, since you made a `types` method with the same signature that it has on `main`. I'm happy to pair on this with you if you want to find some sync time

---
