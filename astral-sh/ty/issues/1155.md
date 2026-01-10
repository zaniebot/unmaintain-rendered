```yaml
number: 1155
title: Enum union and intersection with Any not simplifying correctly
type: issue
state: closed
author: github-actions
labels:
  - bug
  - type properties
  - enums
assignees: []
created_at: 2025-09-09T12:10:06Z
updated_at: 2025-09-23T20:01:23Z
url: https://github.com/astral-sh/ty/issues/1155
synced_at: 2026-01-10T02:06:25Z
```

# Enum union and intersection with Any not simplifying correctly

---

_Issue opened by @github-actions on 2025-09-09 12:10_

Run listed here: https://github.com/astral-sh/ty/actions/runs/17581914325

---

_Label `bug` added by @github-actions[bot] on 2025-09-09 12:10_

---

_Label `type properties` added by @github-actions[bot] on 2025-09-09 12:10_

---

_Comment by @AlexWaygood on 2025-09-09 12:17_

```
failures:

---- types::property_tests::stable::subtype_of_is_antisymmetric stdout ----

thread 'types::property_tests::stable::subtype_of_is_antisymmetric' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [Union([Any, EnumLiteral("unsafe")]), EnumLiteral("unsafe")], neg: [] }, EnumLiteral("unsafe"))
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @sharkdp on 2025-09-09 15:41_

It looks like `(Any | E) & E = Any & E | E` doesn't simplify to `E`, or that it is not considered equivalent to `E`.

(I'm using `E` instead of `Literal[SafeUUID.unsafe]` as a placeholder for some enum literal)

It looks like we *do* consider them equivalent if `E` is a singleton, e.g. if you remove the `B` variant from the enum in https://play.ty.dev/f0d5d174-f462-4dc8-9c97-3e2cb339e09d 

But `Any & E | E = E` should hold for every (fully static) type, I think? `Any & E | E` has a bottom materialization of `E`, and a top materialization of `E`.

---

_Comment by @carljm on 2025-09-09 16:40_

Could this have been caused by https://github.com/astral-sh/ruff/pull/20285 ? I don't think that was intended to change any behavior that would affect this case, but it did touch the definition of `is_single_valued` for enums, and it seems like quite coincidental timing.

---

_Renamed from "Daily property test run failed on Tue Sep 09 2025" to "Enum union and intersection with Any not simplifying correctly" by @carljm on 2025-09-09 16:40_

---

_Assigned to @sharkdp by @sharkdp on 2025-09-09 19:04_

---

_Comment by @sharkdp on 2025-09-09 19:04_

I can take a look tomorrow.

---

_Comment by @sharkdp on 2025-09-10 07:44_

This has nothing to do with the newly merged PR.

It's a bug that has probably been around for two months, since I introduced enum literals originally. I think we just got very lucky here to generate just the (`(Any | E) & E`, `E`) combination with the same enum literal appearing three times.

=> https://github.com/astral-sh/ruff/pull/20324

---

_Closed by @sharkdp on 2025-09-10 13:54_

---

_Label `enums` added by @AlexWaygood on 2025-09-23 20:01_

---
