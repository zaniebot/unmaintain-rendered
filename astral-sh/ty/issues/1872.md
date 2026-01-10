```yaml
number: 1872
title: Equivalence of generic callables
type: issue
state: open
author: carljm
labels:
  - bug
  - generics
  - type properties
assignees: []
created_at: 2025-12-12T23:46:54Z
updated_at: 2025-12-12T23:47:59Z
url: https://github.com/astral-sh/ty/issues/1872
synced_at: 2026-01-10T01:55:00Z
```

# Equivalence of generic callables

---

_Issue opened by @carljm on 2025-12-12 23:46_

We understand assignability and subtyping of generic functions/callables reasonably well, but not equivalence:

```py
from ty_extensions import CallableTypeOf, static_assert, is_subtype_of, is_assignable_to, is_equivalent_to
from typing import TypeVar

def f[T](x: T) -> T:
    return x

def g[T](x: T) -> T:
    return x

_: CallableTypeOf[g] = f
_: CallableTypeOf[f] = g


static_assert(is_equivalent_to(CallableTypeOf[g], CallableTypeOf[f]))
static_assert(is_subtype_of(CallableTypeOf[g], CallableTypeOf[f]))
static_assert(is_subtype_of(CallableTypeOf[f], CallableTypeOf[g]))
static_assert(is_assignable_to(CallableTypeOf[g], CallableTypeOf[f]))
static_assert(is_assignable_to(CallableTypeOf[f], CallableTypeOf[g]))

T = TypeVar("T")
U = TypeVar("U")

def h(x: T) -> T:
    return x

def i(x: U) -> U:
    return x

_: CallableTypeOf[h] = i
_: CallableTypeOf[i] = h

static_assert(is_equivalent_to(CallableTypeOf[h], CallableTypeOf[i]))
static_assert(is_subtype_of(CallableTypeOf[h], CallableTypeOf[i]))
static_assert(is_subtype_of(CallableTypeOf[i], CallableTypeOf[h]))
static_assert(is_assignable_to(CallableTypeOf[h], CallableTypeOf[i]))
static_assert(is_assignable_to(CallableTypeOf[i], CallableTypeOf[i]))
```

https://play.ty.dev/913c181c-03d3-4c0c-98bf-05e9622395b8

All of those assertions should pass. Currently all of them pass except the two `is_equivalent_to` assertions. But mutual subtyping implies equivalence!

I suspect with our current implementation of equivalence, this will require `normalized` also normalizing typevars by reusing a standard set of typevars.

Or the other approach would be to fix this by switching to an implementation of equivalence based on mutual subtyping (of top/bottom materializations, for gradual types).

---

_Added to milestone `Stable` by @carljm on 2025-12-12 23:46_

---

_Label `bug` added by @carljm on 2025-12-12 23:46_

---

_Label `generics` added by @carljm on 2025-12-12 23:46_

---

_Label `type properties` added by @carljm on 2025-12-12 23:46_

---

_Removed from milestone `Stable` by @carljm on 2025-12-12 23:47_

---

_Comment by @carljm on 2025-12-12 23:47_

This is related to #165.

---
