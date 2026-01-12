```yaml
number: 1341
title: "Distinguish \"declared\" and \"inferred\" `Place`s"
type: issue
state: open
author: sharkdp
labels:
  - internal
assignees: []
created_at: 2025-10-13T07:33:50Z
updated_at: 2025-10-14T15:46:56Z
url: https://github.com/astral-sh/ty/issues/1341
synced_at: 2026-01-12T15:54:25Z
```

# Distinguish "declared" and "inferred" `Place`s

---

_@sharkdp_

A `Place` is currently either `Place::Unbound`, `Place::Type(type, Boundness::PossiblyUnbound` or `Place::Type(type, Boundness::Bound)`:

```rs
pub(crate) enum Boundness {
    Bound,
    PossiblyUnbound,
}

pub(crate) enum Place<'db> {
    Type(Type<'db>, Boundness),
    Unbound,
}
```

There is currently no way of tracking if a `Place` originated from a declaration or if its type has been inferred. However, we still use e.g. `Place::Type(type, Boundness::Bound)` to mean "definitely declared" in various places. For example, the `place_from_declarations` query also returns a `Place` (wrapped in `PlaceFromDeclarationsResult` -> `PlaceAndQualifiers` -> `Place`).

Similarly, the `Member` return type of various member-access functions [currently tracks](https://github.com/astral-sh/ruff/blob/9b9c9ae0923762fdea90d1ddd01ee8e7e87064dc/crates/ty_python_semantic/src/member.rs#L14-L18) the "declared vs inferred" information separately.

We should attempt to refactor `Place` to generally contain metadata about the origin of a particular type.

Related issue: #1051 

---

_Label `internal` added by @sharkdp on 2025-10-13 07:33_

---

_Comment by @mtshiba on 2025-10-13 17:40_

I remember considering the related issue during astral-sh/ruff#18041. At the time, it was associated with #229.
The new API for `Place` I was considering at the time was as follows:

```rust
// Boundness renamed
pub(crate) enum Definedness {
    Defined, // Bound renamed
    PossiblyUndefined, // PossiblyUnbound renamed
}

pub(crate) enum Place<'db> {
    // If there is both a declaration and a binding, `Place::Bound` will be used.
    Bound(Type<'db>, Definedness),
    Declared(Type<'db>, Definedness),
    Undefined,
}
```

If no one has started working on this yet, I'll get to it.
Please let me know if you have any comments on the direction of the new API.


---

_Comment by @sharkdp on 2025-10-14 07:09_

>  The new API for Place I was considering at the time was as follows:

Sounds reasonable.

> Please let me know if you have any comments on the direction of the new API.

Another idea would be something like the following with just a single variant that holds a `Type`.
```rs
pub(crate) enum TypeOrigin {
    Declared,
    Inferred,
}

pub(crate) enum Place<'db> {
    // If there is both a declaration and a binding, `Place::Bound` will be used.
    Defined(Type<'db>, TypeOrigin, Definedness),
    Undefined,
}
```
I guess it depends on which representation feels the most ergonomic when actually doing the refactor.

> If no one has started working on this yet, I'll get to it.

That would be great. If it turns out to be a *huge* change that affects several other open PRs, *maybe* it would be better to do this after the beta release? Or maybe there is a smart way of doing this in several smaller steps, where the first step would keep the API of `Place` mostly intact?

---

_Comment by @mtshiba on 2025-10-14 15:43_

> That would be great. If it turns out to be a huge change that affects several other open PRs, maybe it would be better to do this after the beta release? Or maybe there is a smart way of doing this in several smaller steps, where the first step would keep the API of Place mostly intact?

I think we can start by adding `Place::Declared` and replacing all current `Place::Type` with `Place::Bound` (`Place::Inferred` would be a better name than `Place::Bound` for consistency with other existing types such as `DeclaredAndInferredType`), allowing us to introduce the new API without changing behavior.
`Place::Declared` will be used in the next step.

Alternatively, if we will use `Place::{Defined, Undefined}` and `TypeOrigin`, all `TypeOrigin`s will initially be set to `Inferred`.



---
