```yaml
number: 16618
title: "[red-knot] Callable member lookup, meta type impl"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-member-lookup
created_at: 2025-03-11T08:47:35Z
updated_at: 2025-03-12T06:31:39Z
url: https://github.com/astral-sh/ruff/pull/16618
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Callable member lookup, meta type impl

---

_Pull request opened by @dhruvmanila on 2025-03-11 08:47_

## Summary

This PR is a follow-up to https://github.com/astral-sh/ruff/pull/16493 that implements member lookup for the general callable type.

Based on the discussion around [member lookup here](https://github.com/astral-sh/ruff/pull/16493#discussion_r1982041180) and [`.to_meta_type()` here](https://github.com/astral-sh/ruff/pull/16493#discussion_r1985104664).

## Test Plan

Add a new test cases.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-11 08:47_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-11 08:47_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-11 08:47_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-11 08:47_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-11 08:47_

---

_Comment by @github-actions[bot] on 2025-03-11 08:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1609 on 2025-03-11 20:06_

I think this needs to be
```suggestion
                KnownClass::Object.to_instance(db).instance_member(db, name)
```

Probably doesn't matter since you special case general callable types in `member_lookup_with_policy` below, but it would matter if this function gains more call sites.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3170 on 2025-03-11 20:34_

This seems wrong to me. I don't think it's necessarily true that the meta-type of every callable is `type`. For example, a function is a callable, and it's meta-type is not `type`.

It's a bit more difficult that this leads to incorrect behavior, since `member_lookup_with_policy` also special-cases general callables, but the following works.
```py
from inspect import getattr_static
from typing import Callable, reveal_type

def reveal(c: Callable[[], None]):
    reveal_type(getattr_static(c, "mro"))  # Literal[mro], should not exist
```
This reveals `Literal[mro]` on your branch (which comes from class `type`), but the attribute should be non-existent (you would get an `AttributeError` at runtime).

I think this should probably be changed to
```suggestion
            Type::Callable(CallableType::General(_)) => KnownClass::Object.to_class_literal(db),
```

---

_@sharkdp approved on 2025-03-11 20:35_

Thank you for working on this. I think two minor things need to be changed before merging this.

---

_@AlexWaygood reviewed on 2025-03-11 20:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3170 on 2025-03-11 20:55_

I think David is correct here that this is wrong but incorrect on the solution. I think it should be

```suggestion
            Type::Callable(CallableType::General(_)) => KnownClass::Type.instance(db),
```

`KnownClass::Object.to_class_literal(db)` implies that the type of `x.__class__` can be accurately inferred as `Literal[object]` for any `x` where `x` is callable. But that's not true; there are lots of different callable classes and `x.__class__` will be different for all of them.

The most we can say is that for any callable `x`, `x.__class__` will be an instance of `type`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3170 on 2025-03-12 04:45_

I think Alex is right, except its `to_instance` not `instance` :)
```suggestion
            Type::Callable(CallableType::General(_)) => KnownClass::Type.to_instance(db),
```

This is what I'd originally suggested in https://github.com/astral-sh/ruff/pull/16493#discussion_r1985104664, and is what we already do for the (effectively identical in this context) `AlwaysTruthy` and `AlwaysFalsy` types below. (It may even make sense to put these all together in the same branch.)

---

_@carljm reviewed on 2025-03-12 04:45_

---

_@dhruvmanila reviewed on 2025-03-12 05:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:1609 on 2025-03-12 05:52_

Oh right, thanks for catching that.

---

_@dhruvmanila reviewed on 2025-03-12 06:02_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:3170 on 2025-03-12 06:02_

Ugh, thanks for catching that. The main reason I avoided putting it with the mentioned types is that it seemed better to group it along with other callable variants. I'll keep that for now but should be easy to change if that's better.

---

_Merged by @dhruvmanila on 2025-03-12 06:31_

---

_Closed by @dhruvmanila on 2025-03-12 06:31_

---

_Branch deleted on 2025-03-12 06:31_

---
