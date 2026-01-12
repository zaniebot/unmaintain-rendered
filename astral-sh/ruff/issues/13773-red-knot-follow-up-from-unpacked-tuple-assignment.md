```yaml
number: 13773
title: "[red-knot] Follow-up from unpacked tuple assignment work"
type: issue
state: closed
author: dhruvmanila
labels:
  - ty
assignees: []
created_at: 2024-10-16T10:16:30Z
updated_at: 2025-04-19T01:13:18Z
url: https://github.com/astral-sh/ruff/issues/13773
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] Follow-up from unpacked tuple assignment work

---

_@dhruvmanila_

As https://github.com/astral-sh/ruff/pull/13316 is merged, there are some follow-up work that needs to be done. The following is a list in the order of priority where the highest priority work is on the top:

- [x] Avoiding duplicate diagnostics. Considering `(a, b) = (1, 2)`, as the inference runs for each symbol, if there are any diagnostics that's raised during that part (like "too many values to unpack"), then there will be multiple diagnostics added to the builder one for each symbol. Refer https://github.com/astral-sh/ruff/pull/13316#discussion_r1800219731
- [x] Use the unpacking logic in other places where it's relevant like
	- [x] `for` loops e.g., `for (x, y) in ((1, 2), (3, 4)): ...` (#15058)
	- [x] `with` statements e.g., `with (item1, item2) as (x, y): ...`
	- [x] Comprehensions e.g., `[_ for (x, y) in [(1, 2), (3, 4)]]`
- [x] Additional diagnostics when the number of targets is not equal to the number of elements on the RHS (#15086)

---

_Label `red-knot` added by @dhruvmanila on 2024-10-16 10:16_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-10-24 03:34_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:17_

---

_Renamed from "Follow-up from unpacked tuple assignment work" to "[red-knot] Follow-up from unpacked tuple assignment work" by @carljm on 2024-11-07 15:34_

---

_Comment by @dhruvmanila on 2024-11-18 05:10_

Regarding "Use the unpacking logic in other places where it's relevant like" task, I think the challenge here would be to implement the logic to "combine" union types into a single type. For example, in a `for` loop like:
```py
for (x, y) in ((1, 2), (3, 4), (5, 6)): ...
```
The inferred type for the iterable would be:
```py
tuple[Literal[1], Literal[2]] | tuple[Literal[3], Literal[4]] | tuple[Literal[5], Literal[6]]
```
But, we should combine it to:
```py
tuple[int, int]
# or
tuple[Literal[1, 3, 5], Literal[2, 4, 6]]
```
Or, another example where the types are "mixed":
```py
for (x, y) in ((1, 2), ("a", "b")): ...
```
We should combine it to:
```py
tuple[int | str, int | str]
# or
tuple[Literal[1, "a"], Literal[2, "b"]]
```
Another example:
```py
for (x, y) in ((1, 2), ("a", 3)): ...
```
where,
```py
tuple[int | str, int]
# or
tuple[Literal[1, "a"], Literal[2, 3]]
```

---

_Comment by @dhruvmanila on 2024-12-17 10:07_

Actually, my previous comment isn't exactly correct. We actually need to add support for union types when unpacking rather than combining the types.

For example, if the type is `tuple[int, str] | list[int]` and the target has two elements `(a, b)`, then the type of `a` will be a union of the first tuple type (`int`) and the iterable type of the list (`int`) which should be `int`. And, similarly the type of `b` would be a union of the second tuple type (`str`) and the iterable type of the list (`int`) which would be `int | str`.

cc @carljm as we talked about this in our 1:1 yesterday.

---

_Removed from milestone `Red Knot 2024` by @carljm on 2025-01-09 17:43_

---

_Comment by @carljm on 2025-02-24 19:28_

@dhruvmanila What's the status here? I think I recall from a past conversation that you had planned to close this issue and create dedicated new issues for future follow-up on the remaining not-completed items?

---

_Comment by @dhruvmanila on 2025-02-25 10:37_

Thanks for the reminder. I've extracted the remaining entries from the checklist into their own sub-issues. I'll close this now.

---

_Closed by @dhruvmanila on 2025-02-25 10:37_

---
