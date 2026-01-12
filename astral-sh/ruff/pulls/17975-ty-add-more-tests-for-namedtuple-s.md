```yaml
number: 17975
title: "[ty] Add more tests for `NamedTuple`s"
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: dataclass
created_at: 2025-05-09T06:44:38Z
updated_at: 2025-05-10T08:46:08Z
url: https://github.com/astral-sh/ruff/pull/17975
synced_at: 2026-01-12T15:56:09Z
```

# [ty] Add more tests for `NamedTuple`s

---

_@abhijeetbodas2001_

## Summary

Add more tests and TODOs for `NamedTuple` support, based on the typing spec: https://typing.python.org/en/latest/spec/namedtuples.html

## Test Plan

This PR adds new tests.


---

_Review requested from @carljm by @abhijeetbodas2001 on 2025-05-09 06:44_

---

_Review requested from @AlexWaygood by @abhijeetbodas2001 on 2025-05-09 06:44_

---

_Review requested from @sharkdp by @abhijeetbodas2001 on 2025-05-09 06:44_

---

_Review requested from @dcreager by @abhijeetbodas2001 on 2025-05-09 06:44_

---

_Comment by @abhijeetbodas2001 on 2025-05-09 06:45_

@sharkdp I was going through the #17738 and found that we do not yet enforce these checks.
Since this might be less of a priority right now, I added test TODOs instead of opening an issue.
Could you review?

---

_Comment by @github-actions[bot] on 2025-05-09 06:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-09 06:48_

@sharkdp is out today and Monday. It might take him a bit longer to take a look

---

_Label `ty` added by @MichaReiser on 2025-05-09 06:48_

---

_@sharkdp approved on 2025-05-09 07:07_

Thank you!

---

_@sharkdp reviewed on 2025-05-09 13:31_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:120 on 2025-05-09 13:31_

This is actually not an error at runtime. It's confusing, maybe (and something that a linter might catch), but I don't think we should emit an error here? Or what did you have in mind?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:17 on 2025-05-09 13:32_

Can we move this test somewhere below? I'd like to have the "Basics" test on top.

---

_@sharkdp reviewed on 2025-05-09 13:32_

---

_Comment by @abhijeetbodas2001 on 2025-05-09 17:13_

Thanks David. I've addressed both your comments.

---

_@abhijeetbodas2001 reviewed on 2025-05-09 17:14_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:120 on 2025-05-09 17:14_

The spec expects conflicting fields to be caught by type checkers:

> A named tuple class can be subclassed, but any fields added by the subclass are not considered part of the named tuple type. Type checkers should enforce that these newly-added fields do not conflict with the named tuple fields in the base class

Pyright does flag this, for example.

This can lead to unexpected / weird runtime behaviour:

```py
[nav] In [8]: class Point(NamedTuple):
         ...:     x: int
         ...:     y: int
         ...:     units: str = "meters"

[nav] In [9]: class PointWithName(Point):
         ...:     name: str  # OK
         ...:     x: int  # repeated field

[ins] In [10]: x = PointWithName("foo", 3)

[ins] In [11]: x.x
Out[11]: 'foo'
```

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:17 on 2025-05-09 17:14_

Sure, done!

---

_@abhijeetbodas2001 reviewed on 2025-05-09 17:14_

---

_@sharkdp reviewed on 2025-05-10 08:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:120 on 2025-05-10 08:45_

Thanks — I missed this.

---

_Label `testing` added by @sharkdp on 2025-05-10 08:46_

---

_Merged by @sharkdp on 2025-05-10 08:46_

---

_Closed by @sharkdp on 2025-05-10 08:46_

---
