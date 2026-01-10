```yaml
number: 239
title: Implement Homogeneous Tuple Type
type: issue
state: closed
author: cake-monotone
labels: []
assignees: []
created_at: 2024-10-21T11:02:55Z
updated_at: 2025-05-09T16:48:20Z
url: https://github.com/astral-sh/ty/issues/239
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

_Renamed from "[red-knot] Implement Homogeneous Tuple Type" to "Implement Homogeneous Tuple Type" by @MichaReiser on 2025-05-07 15:27_

---

_Comment by @MichaReiser on 2025-05-09 16:48_

Not sure what went wrong here but this is a duplicate of https://github.com/astral-sh/ty/issues/237

---

_Closed by @MichaReiser on 2025-05-09 16:48_

---
