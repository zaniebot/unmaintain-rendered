```yaml
number: 2363
title: "Intersection of `~type` and `~Callable[..., Unknown]` fails `all_type_pairs_are_assignable_to_their_union`"
type: issue
state: closed
author: github-actions
labels:
  - bug
  - type properties
assignees: []
created_at: 2026-01-06T12:13:22Z
updated_at: 2026-01-07T17:18:41Z
url: https://github.com/astral-sh/ty/issues/2363
synced_at: 2026-01-10T01:56:41Z
```

# Intersection of `~type` and `~Callable[..., Unknown]` fails `all_type_pairs_are_assignable_to_their_union`

---

_Issue opened by @github-actions on 2026-01-06 12:13_

Run listed here: https://github.com/astral-sh/ty/actions/runs/20747681194

---

_Label `bug` added by @github-actions[bot] on 2026-01-06 12:13_

---

_Label `type properties` added by @github-actions[bot] on 2026-01-06 12:13_

---

_Comment by @MichaReiser on 2026-01-06 12:21_

```
failures:

---- types::property_tests::stable::all_type_pairs_are_assignable_to_their_union stdout ----

Failing types were:
~type & ~Literal[b""] & ~((...) -> Unknown)
~type | <class 'ABC'>

Failing types were:
~type & ~((...) -> Unknown)
~type | <class 'ABC'>

Failing types were:
~type & ~((...) -> Unknown)
~type | <class 'ABC'>

Failing types were:
~type & ~((...) -> Unknown)
~type

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' (4472) panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [], neg: [BuiltinInstance("type"), Callable { params: GradualForm, returns: None }] }, Intersection { pos: [], neg: [BuiltinInstance("type")] })

```

---

_Comment by @AlexWaygood on 2026-01-06 12:21_

```
failures:

---- types::property_tests::stable::all_type_pairs_are_assignable_to_their_union stdout ----

<-- snip -->

Failing types were:
~type & ~((...) -> Unknown)
~type

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' (4472) panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [], neg: [BuiltinInstance("type"), Callable { params: GradualForm, returns: None }] }, Intersection { pos: [], neg: [BuiltinInstance("type")] })
```

---

_Comment by @AlexWaygood on 2026-01-06 12:36_

I'm struggling to reproduce this. All these assertions pass for me [in the playground](https://play.ty.dev/f1b722bb-c296-4ef5-b694-fe24fefdb9cd):

```py
from ty_extensions import Not, Intersection, is_assignable_to, Unknown, static_assert
from typing import Callable, reveal_type

static_assert(is_assignable_to(Not[type], Not[type] | Intersection[Not[type], Not[Callable[..., Unknown]]]))
static_assert(is_assignable_to(Not[type], Intersection[Not[type], Not[Callable[..., Unknown]]] | Not[type]))
static_assert(is_assignable_to(Not[type], Not[type] | Intersection[Not[Callable[..., Unknown]], Not[type]]))
static_assert(is_assignable_to(Not[type], Intersection[Not[Callable[..., Unknown]], Not[type]] | Not[type]))

static_assert(is_assignable_to(Intersection[Not[type], Not[Callable[..., Unknown]]], Not[type] | Intersection[Not[type], Not[Callable[..., Unknown]]]))
static_assert(is_assignable_to(Intersection[Not[Callable[..., Unknown]], Not[type]], Not[type] | Intersection[Not[Callable[..., Unknown]], Not[type]]))
static_assert(is_assignable_to(Intersection[Not[type], Not[Callable[..., Unknown]]], Intersection[Not[type], Not[Callable[..., Unknown]]] | Not[type]))
static_assert(is_assignable_to(Intersection[Not[Callable[..., Unknown]], Not[type]], Intersection[Not[Callable[..., Unknown]], Not[type]] | Not[type]))

type A = Not[type]
type B = Intersection[Not[type], Not[Callable[..., Unknown]]]

static_assert(is_assignable_to(A, A | B))
static_assert(is_assignable_to(A, B | A))
static_assert(is_assignable_to(B, A | B))
static_assert(is_assignable_to(B, B | A))
```

---

_Comment by @carljm on 2026-01-06 18:16_

It looks like the bug is specific to callable types with no return annotation (as distinct from callable types with an explicit return annotation of `Unknown`). Apparently this makes a difference somewhere, even though these should be equivalent and we display them the same. So it requires `CallableTypeOf` to [reproduce in Python](https://play.ty.dev/0695b057-1336-4ef5-afc9-d93319f4f6aa):

```py
from ty_extensions import static_assert, is_assignable_to, Intersection, Not, Unknown, CallableTypeOf
from typing import Callable

# This passes because `Callable[..., Unknown]` has an explicit Unknown return type
static_assert(is_assignable_to(Intersection[Not[type], Not[Callable[..., Unknown]]], Not[type]))

# Function with no return annotation (has implicit Unknown return type internally)
def no_return_annotation(*args, **kwargs): ...

# This fails:
static_assert(is_assignable_to(Intersection[Not[type], Not[CallableTypeOf[no_return_annotation]]], Not[type]))
```

(Full disclosure, Claude tracked this down)

---

_Comment by @AlexWaygood on 2026-01-06 18:25_

Looks like a regression introduced in ty@0.0.7; the assertion passes on ty@0.0.6.

---

_Comment by @AlexWaygood on 2026-01-06 18:29_

The regression bisects to https://github.com/astral-sh/ruff/pull/22145 -- cc. @charliermarsh 

---

_Comment by @carljm on 2026-01-06 18:31_

I've already got Claude working on a fix

---

_Comment by @AlexWaygood on 2026-01-06 18:36_

It seems like quite the footgun that we represent "no return type" differently to "explicitly returns `Unknown`", honestly -- is there any reason for that?

Skimming through places where the `Signature::return_ty` field is accessed, I think I can see ~5 possible bugs or so due to us incorrectly treating "no return type" differently to "explicitly returns `Unknown`"

---

_Comment by @carljm on 2026-01-06 18:43_

I remember debating about it in the early days of callable signature representation. I don't think there was a strong reason -- it seemed better not to throw away information, but probably it would be better to throw it away, if it should never make a difference.

It looks like the bug here has to do with materialization of a callable with (Rust) `None` return not being handled correctly.

---

_Renamed from "Daily property test run failed on Tue Jan 06 2026" to "Intersection of `~type` and `~Callable[..., Unknown]` fails `all_type_pairs_are_assignable_to_their_union`" by @carljm on 2026-01-06 18:44_

---

_Comment by @carljm on 2026-01-06 18:49_

I have a narrow fix working here for the materialization problem -- before I put it up, going to try the broader fix of making the return type non-optional and eagerly inserting `Unknown` for no-return-annotation.

---

_Comment by @carljm on 2026-01-07 00:58_

> I think I can see ~5 possible bugs or so

You were not wrong. I've so far discovered two additional bugs fixed by https://github.com/astral-sh/ruff/pull/22425, and added tests for them. So the count stands at 3; we'll see if I get to 5.

---

_Closed by @carljm on 2026-01-07 17:18_

---
