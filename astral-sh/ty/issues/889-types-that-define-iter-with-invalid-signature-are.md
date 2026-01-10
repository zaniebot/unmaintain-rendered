```yaml
number: 889
title: "Types that define `__iter__` with invalid signature are wrongly considered assignable to Iterable"
type: issue
state: closed
author: github-actions
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-07-25T12:06:05Z
updated_at: 2025-09-12T10:10:33Z
url: https://github.com/astral-sh/ty/issues/889
synced_at: 2026-01-10T02:06:24Z
```

# Types that define `__iter__` with invalid signature are wrongly considered assignable to Iterable

---

_Issue opened by @github-actions on 2025-07-25 12:06_

Run listed here: https://github.com/astral-sh/ty/actions/runs/16521468852

---

_Label `bug` added by @github-actions[bot] on 2025-07-25 12:06_

---

_Label `type properties` added by @github-actions[bot] on 2025-07-25 12:06_

---

_Comment by @AlexWaygood on 2025-07-25 12:08_

```
---- types::property_tests::stable::all_type_assignable_to_iterable_are_iterable stdout ----

thread 'types::property_tests::stable::all_type_assignable_to_iterable_are_iterable' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (EnumLiteral("unsafe"))
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    types::property_tests::stable::all_type_assignable_to_iterable_are_iterable
```

A merge race between https://github.com/astral-sh/ruff/pull/19187 and https://github.com/astral-sh/ruff/pull/19328, I'd guess.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-07-25 12:16_

---

_Comment by @sharkdp on 2025-07-25 12:19_

> AlexWaygood self-assigned this

Thanks! Let me know if I can be of help

> A merge race between [astral-sh/ruff#19187](https://github.com/astral-sh/ruff/pull/19187) and [astral-sh/ruff#19328](https://github.com/astral-sh/ruff/pull/19328), I'd guess.

A bit of a stretch to call it a race: the enum literals PR was merged 10 days ago 😜 

---

_Comment by @AlexWaygood on 2025-07-25 12:22_

the issue is _fairly_ obvious: `type(uuid.UUID.unsafe).__iter__` returns a bound attribute at runtime, and we recognize it as such in ty. But that's not sufficient for `uuid.UUID.unsafe` to be iterable, because the signature of `type(uuid.UUID.unsafe).__iter__` doesn't match the signature specified by the `typing.Iterable` protocol (its `Callable` supertype is `() -> Iterator` rather than `(self) -> Iterator`).

So this is just a missing `Protocol` feature, and it's the next one I was going to tackle anyway now that https://github.com/astral-sh/ruff/pull/19187 has landed!

---

_Renamed from "Daily property test run failed on Fri Jul 25 2025" to "Some types that define `__iter__` with invalid signature are wrongly considered assignable to Iterable" by @carljm on 2025-07-25 15:23_

---

_Renamed from "Some types that define `__iter__` with invalid signature are wrongly considered assignable to Iterable" to "Types that define `__iter__` with invalid signature are wrongly considered assignable to Iterable" by @carljm on 2025-07-25 15:23_

---

_Added to milestone `Beta` by @carljm on 2025-07-25 15:23_

---

_Closed by @AlexWaygood on 2025-09-12 10:10_

---
