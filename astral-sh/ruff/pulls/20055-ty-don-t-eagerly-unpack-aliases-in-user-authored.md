```yaml
number: 20055
title: "[ty] don't eagerly unpack aliases in user-authored unions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/aliasinunion
created_at: 2025-08-23T13:06:26Z
updated_at: 2025-08-26T23:29:46Z
url: https://github.com/astral-sh/ruff/pull/20055
synced_at: 2026-01-10T17:46:21Z
```

# [ty] don't eagerly unpack aliases in user-authored unions

---

_Pull request opened by @carljm on 2025-08-23 13:06_

## Summary

Add a subtly different test case for recursive PEP 695 type aliases, which does require that we relax our union simplification, so we don't eagerly unpack aliases from user-provided union annotations.

## Test Plan

Added mdtest.


---

_Label `ty` added by @carljm on 2025-08-23 13:06_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-23 13:06_

---

_Review requested from @sharkdp by @carljm on 2025-08-23 13:06_

---

_Review requested from @dcreager by @carljm on 2025-08-23 13:06_

---

_Comment by @github-actions[bot] on 2025-08-23 13:08_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-23 13:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:225 on 2025-08-23 15:29_

This sort-of feels like a bit of a confusing test to me, because it's not really something anybody would ever write. If you're able to use PEP-695 syntax in your Python code, then you'd surely also use PEP-604 syntax. And if you're using PEP-695 syntax, you'd also surely make use of the fact that the r.h.s. of a PEP-695 alias is lazily evaluated (no need to quote any forward references).

```suggestion
type MarkerAtom = int | list[MarkerAtom]
type MarkerList = list[MarkerList | MarkerAtom | str]

def f(marker_list: MarkerList):
    reveal_type(marker_list)  # revealed: list[MarkerList | MarkerAtom | str]
    for item in marker_list:
        reveal_type(item)  # revealed: list[MarkerList | MarkerAtom | str] | int | list[MarkerAtom] | str
```

But it seems like we still panic on ^that rewritten test on this branch. (It looks like it's the switch to PEP-604 unions that makes us panic; we still run fine on that snippet if all I do is unquote the forward references.)

I'm okay with landing this test as-is (or with just the forward references unquoted) since it definitely demonstrates us making progress towards the goal of full support for recursive type aliases. But it might be worth adding some prose that says why it is the way it is?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:236 on 2025-08-23 15:30_

don't think it really makes much of a difference, but I'd always do this where it's possible

```suggestion
    const fn should_simplify_full(&self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:10558 on 2025-08-23 15:34_

I think you also need to add this handling to `Optional` -- this test case still causes us to panic on your branch:

```py
from typing import Optional, Union

type MarkerAtom = Optional[list["MarkerAtom"]]
type MarkerList = list[Optional[Union["MarkerList", MarkerAtom, str]]]

def f(marker_list: MarkerList):
    reveal_type(marker_list)
    for item in marker_list:
        reveal_type(item)
```

---

_@AlexWaygood approved on 2025-08-23 15:36_

🚀

---

_@AlexWaygood reviewed on 2025-08-23 16:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:207 on 2025-08-23 16:54_

```suggestion
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
enum SimplificationStrategy {
```

---

_@carljm reviewed on 2025-08-23 17:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:225 on 2025-08-23 17:59_

Ugh, silly oversight -- I just always need to write tests for all forms of the syntax. Good catch! No, I'll fix this in this PR. But I want to include both tests, even the "weird" one -- this is fixing things that will be needed for other kinds of type aliases, too, but I want it tested for all combinations, even the weird ones.

---

_Converted to draft by @carljm on 2025-08-23 23:48_

---

_Renamed from "[ty] fix a recursive alias case via less eager union simplification" to "[ty] don't eagerly unpack aliases in user-authored unions" by @carljm on 2025-08-26 22:35_

---

_Marked ready for review by @carljm on 2025-08-26 23:28_

---

_Merged by @carljm on 2025-08-26 23:29_

---

_Closed by @carljm on 2025-08-26 23:29_

---

_Branch deleted on 2025-08-26 23:29_

---
