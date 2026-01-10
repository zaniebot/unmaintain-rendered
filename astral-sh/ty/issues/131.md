```yaml
number: 131
title: "Generic classes with dynamic types (e.g. `A[Unknown]`) should not be considered fully static"
type: issue
state: closed
author: cake-monotone
labels:
  - bug
  - generics
  - type properties
assignees: []
created_at: 2025-04-10T04:51:40Z
updated_at: 2025-07-01T06:41:56Z
url: https://github.com/astral-sh/ty/issues/131
synced_at: 2026-01-10T02:07:35Z
```

# Generic classes with dynamic types (e.g. `A[Unknown]`) should not be considered fully static

---

_Issue opened by @cake-monotone on 2025-04-10 04:51_

https://playknot.ruff.rs/73be425a-e02f-425f-b4dd-4eb70eb3fe8e

```py
from knot_extensions import is_fully_static, static_assert, TypeOf
from typing import reveal_type

class A[T]: ...

a_unknown = A()
reveal_type(a_unknown)  # revealed: A[Unknown]

static_assert(not is_fully_static(A))
static_assert(not is_fully_static(TypeOf[a_unknown]))
```

<details>
<summary>Outdated</summary>

https://playknot.ruff.rs/e59ddd94-b32c-4275-b038-42a1f8fc1823

```py
from knot_extensions import is_subtype_of, static_assert, TypeOf
from typing import reveal_type

class A[T]: ...

a_unknown = A()
a_int = A[int]()

reveal_type(a_unknown)  # revealed: A[Unknown]
reveal_type(a_int)  # revealed: A[int]

static_assert(is_subtype_of(A[int], A))
static_assert(is_subtype_of(TypeOf[a_int], TypeOf[a_unknown]))
```

It seems like there's not yet proper support for subtype relationships in this case.
Considering the Liskov Substitution Principle, I guess `A[int]` should be a subtype of `A[Unknown]`.

</details>

---

_Comment by @AlexWaygood on 2025-04-10 10:45_

I don't think `A[int]` should be considered a subtype of `A[Unknown]` because `A[Unknown]` isn't a fully static type, so it doesn't participate in subtyping at all. https://typing.python.org/en/latest/spec/concepts.html# is the long article in the typing spec describing all this in depth -- https://typing.python.org/en/latest/spec/glossary.html#term-subtype, https://typing.python.org/en/latest/spec/glossary.html#term-assignable, and https://typing.python.org/en/latest/spec/glossary.html#term-fully-static-type are some slightly shorter definitions

`A[int]` _should_ be considered _assignable_ to `A[Unknown]`, however!

---

_Comment by @cake-monotone on 2025-04-10 11:37_

Ah..! Thanks for your kind answer. That insight was really helpful :)
I see now. the issue must be that `A[Unknown]` isn't considered a fully static. (even though the subtype is still false) I mistakenly thought that `A` (with generic) would still be fully static.

---

_Comment by @cake-monotone on 2025-04-10 11:43_

Hmm, I’ll update this issue to focus on fully static rather than subtype

---

_Comment by @cake-monotone on 2025-04-10 11:45_

For now, `is_fully_static(A[Unknown])` is true; it should be false 

---

_Comment by @AlexWaygood on 2025-04-10 11:47_

(cc. @dcreager who has been leading our work on generics recently)

---

_Renamed from "[red-knot] Add proper `is_subtype_of` support for generic classes" to "[red-knot] Generic classes with dynamic types (e.g. `A[Unknown]`) should not be considered fully static" by @cake-monotone on 2025-04-10 11:48_

---

_Renamed from "[red-knot] Generic classes with dynamic types (e.g. `A[Unknown]`) should not be considered fully static" to "Generic classes with dynamic types (e.g. `A[Unknown]`) should not be considered fully static" by @MichaReiser on 2025-05-07 15:25_

---

_Label `bug` added by @AlexWaygood on 2025-05-10 18:06_

---

_Label `generics` added by @AlexWaygood on 2025-05-10 18:06_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:36_

---

_Comment by @carljm on 2025-07-01 06:41_

I don't think this is relevant anymore, now that `is_fully_static` is gone.

---

_Closed by @carljm on 2025-07-01 06:41_

---
