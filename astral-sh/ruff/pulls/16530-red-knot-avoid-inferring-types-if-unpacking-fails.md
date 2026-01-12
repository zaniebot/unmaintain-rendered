```yaml
number: 16530
title: "[red-knot] avoid inferring types if unpacking fails"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-15199-invalid-unpacking
created_at: 2025-03-06T10:31:13Z
updated_at: 2025-03-09T03:57:51Z
url: https://github.com/astral-sh/ruff/pull/16530
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] avoid inferring types if unpacking fails

---

_@mtshiba_

## Summary

This PR closes #15199.

The change I just made is to set all variables to type `Unknown` if unpacking fails, but in some cases this may be excessive.
For example:

```py
a, b, c = "ab"
reveal_type(a)  # Unknown, but it would be reasonable to think of it as LiteralString
reveal_type(c)  # Unknown
```

```py
# Failed to unpack before the starred expression
(a, b, *c, d, e) = (1,)
reveal_type(a)  # Unknown
reveal_type(b)  # Unknown
...
# Failed to unpack after the starred expression
(a, b, *c, d, e) = (1, 2, 3)
reveal_type(a)  # Unknown, but should it be Literal[1]?
reveal_type(b)  # Unknown, but should it be Literal[2]?
reveal_type(c)  # Todo
reveal_type(d)  # Unknown
reveal_type(e)  # Unknown
```

I will modify it if you think it would be better to make it a different type than just `Unknown`.

## Test Plan

I have made appropriate modifications to the test cases affected by this change, and also added some more test cases.


---

_Review requested from @carljm by @mtshiba on 2025-03-06 10:31_

---

_Review requested from @MichaReiser by @mtshiba on 2025-03-06 10:31_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-06 10:31_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-06 10:31_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-06 10:44_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:65 on 2025-03-07 09:44_

We should probably move this comment at the top as it's related to multiple test cases. I'd also remove the behavior comparison from the document.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:93 on 2025-03-07 09:46_

It might be worth expanding your previous comment to state that this behavior is only for the _current_ unpacking and if there are any unpacking in the outer context or a nested unpacking which are valid, then the inference still happens correctly.

This matches the behavior of mypy: https://mypy-play.net/?mypy=latest&python=3.13&gist=d1986f8345de346c91188f530b71a3e9&flags=show-traceback,strict.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:260 on 2025-03-07 09:49_

I think this should be `Unknown` as well. Otherwise, once this todo is fixed, we might start inferring it as `list[Unknown]` which could seem like a special case.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:261 on 2025-03-07 09:59_

We can simplify this entire branch because (a) we want to avoid the `list[Unknown]` as I mentioned above and (b) every element would need to be `Unknown`.

So, we should keep the error reporting and then directly create a vector of `Unknowns` that is exactly equal to the number of targets:
```rs
return Cow::Owned(vec![Type::unknown(); targets.len()]);
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:124 on 2025-03-07 10:05_

I think the value of this is not exactly what it represents because in `tuple_ty_elements` method, we would return a vector of unknowns that's exactly the same length as the number of targets but that's mainly because the error message is a little bit different in that case.

I don't think there's any action item here except that I'd probably rename this variable to something like `length_mismatch`.

---

_@dhruvmanila requested changes on 2025-03-07 10:05_

Thank you for doing this! There are couple of changes around simplification and using `Unknown` even in the case of starred unpacking but otherwise this looks good.

---

_Review requested from @dhruvmanila by @mtshiba on 2025-03-07 18:19_

---

_@carljm approved on 2025-03-07 19:03_

Looks good to me with the changes from Dhruv's review!

---

_Merged by @carljm on 2025-03-07 19:04_

---

_Closed by @carljm on 2025-03-07 19:04_

---

_Branch deleted on 2025-03-09 03:57_

---
