```yaml
number: 165
title: Explore alternate implementation for callable type equivalence
type: issue
state: open
author: dhruvmanila
labels:
  - type properties
assignees: []
created_at: 2025-03-21T03:35:01Z
updated_at: 2025-11-18T16:10:20Z
url: https://github.com/astral-sh/ty/issues/165
synced_at: 2026-01-10T02:06:24Z
```

# Explore alternate implementation for callable type equivalence

---

_Issue opened by @dhruvmanila on 2025-03-21 03:35_

Currently, the general callable type uses a manual implementation to check whether the two types are equivalent (`is_equivalent_to`). This is required for two reasons:
1. The type of the default values don't participate in checking for equivalence, it's only the optionality that participates i.e., is the parameter required for both callable types or not?
2. The name of positional-only, variadic and keyword-variadic parameters don't participate in checking for equivalence

Refer to https://github.com/astral-sh/ruff/pull/16698#discussion_r2003121350 for previous discussion.

A possible option that's discussed in the above thread is to erase the information that doesn't participate in equivalence relation (above two points) when [constructing a `GeneralCallableType` from a function literal](https://github.com/astral-sh/ruff/blob/3b1e030fbbe586a2b8918d4f1c33cea710faa446/crates/red_knot_python_semantic/src/types.rs#L4375-L4383). This will mean that we can remove the manual implementation and it will then use the catch-all branch:

https://github.com/astral-sh/ruff/blob/3b1e030fbbe586a2b8918d4f1c33cea710faa446/crates/red_knot_python_semantic/src/types.rs#L913-L913

This has the benefit that the equivalence is maintained at the type level. But, this does mean that (a) we'll need to make `name: Name` into `name: Option<Name>` to erase it or replace it with `Name("")` (empty name seems error prone) and (b) use something else for the default type as `Type::Unknown` is not a static type, maybe just replace it with `None`.

---

_Comment by @dhruvmanila on 2025-03-24 18:17_

This might not be possible given that we now use the manual implementation for both `is_equivalent_to` and `is_gradual_equivalent_to` (https://github.com/astral-sh/ruff/pull/16887).

---

_Comment by @dhruvmanila on 2025-05-02 14:06_

> A possible option that's discussed in the above thread is to erase the information that doesn't participate in equivalence relation (above two points) when [constructing a `GeneralCallableType` from a function literal](https://github.com/astral-sh/ruff/blob/3b1e030fbbe586a2b8918d4f1c33cea710faa446/crates/red_knot_python_semantic/src/types.rs#L4375-L4383). This will mean that we can remove the manual implementation and it will then use the catch-all branch:

For context, this is possible now via the `normalized` method but that can only be used for `is_equivalent_to`. Refer to https://github.com/astral-sh/ruff/pull/17145#discussion_r2025123895 for why it's not done that way currently.

---

_Renamed from "[red-knot] Explore alternate implementation for callable type equivalence" to "Explore alternate implementation for callable type equivalence" by @MichaReiser on 2025-05-07 15:26_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:30_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:09_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
