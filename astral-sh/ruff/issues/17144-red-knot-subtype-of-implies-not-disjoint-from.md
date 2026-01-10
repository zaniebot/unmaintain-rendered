---
number: 17144
title: "[red-knot] `subtype_of_implies_not_disjoint_from` property test is now flaky"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - testing
  - ty
assignees: []
created_at: 2025-04-02T09:36:33Z
updated_at: 2025-04-02T22:08:14Z
url: https://github.com/astral-sh/ruff/issues/17144
synced_at: 2026-01-10T01:22:58Z
---

# [red-knot] `subtype_of_implies_not_disjoint_from` property test is now flaky

---

_Issue opened by @AlexWaygood on 2025-04-02 09:36_

I got this property test failure on a local PR branch:

```
failures:

---- types::property_tests::stable::subtype_of_implies_not_disjoint_from stdout ----

thread 'types::property_tests::stable::subtype_of_implies_not_disjoint_from' panicked at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [Callable { params: List([Param { kind: PositionalOnly, name: Some(Name("n1")), annotated_ty: Some(KnownClassInstance(Object)), default_ty: None }]), returns: Some(StringLiteral("")) }, StringLiteral("")], neg: [] }, LiteralString)
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Which I minimized down to this:

```py
from typing import Callable, Literal, LiteralString
from knot_extensions import Intersection, static_assert, is_subtype_of, is_disjoint_from

static_assert(is_subtype_of(Intersection[Callable[[], None], Literal[""]], LiteralString))
static_assert(not is_disjoint_from(Intersection[Callable[[], None], Literal[""]], LiteralString))  # This assertion fails, but should logically hold true if the first assertion passes!
```

And the minimal repro also reproduces on `main`: https://playknot.ruff.rs/8cad645a-b986-42b8-baa7-ef8b75e81b3e

cc. @dhruvmanila

---

_Label `bug` added by @AlexWaygood on 2025-04-02 09:36_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-02 09:36_

---

_Label `testing` added by @AlexWaygood on 2025-04-02 09:36_

---

_Comment by @dhruvmanila on 2025-04-02 16:06_

(Acknowledging, I'll look into it today. Thanks for the repro!)

---

_Comment by @AlexWaygood on 2025-04-02 18:06_

I had a thought -- I think the issue (or at least one issue) might be that we should probably understand `Literal[""]` as disjoint from all `Callable` types, but we currently don't?

We know that for all `StringLiteral` types, their `__class__` is set to _exactly_ `str`. Not "`str` or some subclass of `str`" -- _exactly_ `str`. And instances of `str` are not callable; therefore all `StringLiteral` types are disjoint from all callable types. The same applies to all `Type::IntLiteral`, `Type::BooleanLiteral`, `Type::BytesLiteral` and `Type::SliceLiteral` types.

If we understand these variants as being disjoint from `Type::Callable` types, I think this will mean that we'll simplify `Callable[[], None] & Literal[""]` to `Never`, which should fix the property test.

---

_Referenced in [astral-sh/ruff#17160](../../astral-sh/ruff/pulls/17160.md) on 2025-04-02 21:21_

---

_Closed by @dhruvmanila on 2025-04-02 22:08_

---
