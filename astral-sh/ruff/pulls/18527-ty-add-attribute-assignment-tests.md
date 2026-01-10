```yaml
number: 18527
title: "[ty] Add attribute assignment tests"
type: pull_request
state: closed
author: thejchap
labels:
  - ty
assignees: []
draft: true
base: main
head: thejchap/union-attribute-assignment-possibly-undefined
created_at: 2025-06-07T02:31:22Z
updated_at: 2025-07-20T23:53:18Z
url: https://github.com/astral-sh/ruff/pull/18527
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Add attribute assignment tests

---

_Pull request opened by @thejchap on 2025-06-07 02:31_

## Summary
Related to: https://github.com/astral-sh/ruff/pull/18347

This PR adds tests for some of the current behavior of `validate_attribute_assignment`, to help with the refactor. This catches some issues that surfaced in the ecosystem check.

Opening as a separate PR so I can see these pass against `main` then rebase https://github.com/astral-sh/ruff/pull/18347 on top of this and fail them

### Projects
- [ ] beartype
- [ ] datadog
- [ ] sphinx
  - https://sourceforge.net/p/docutils/code/HEAD/tree/trunk/docutils/docutils/frontend.py#l568 conditional initialization
  - https://github.com/sphinx-doc/sphinx/blob/master/sphinx/builders/__init__.py#L689
- [ ] streamlit

## Test Plan
New mdtests

---

_@thejchap reviewed on 2025-06-07 02:32_

---

_Review comment by @thejchap on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1669 on 2025-06-07 02:32_

mypy:

`../test.py:12: error: Item "A" of "A | B" has no attribute "x"  [union-attr]`

---

_Renamed from "[ty] Add attribute assignment tests for unions" to "[ty] Add attribute assignment tests" by @thejchap on 2025-06-07 02:37_

---

_Comment by @github-actions[bot] on 2025-06-07 02:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@thejchap reviewed on 2025-06-07 02:55_

---

_Review comment by @thejchap on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1784 on 2025-06-07 02:55_

~this should address the [beartype regression](https://github.com/astral-sh/ruff/pull/18347#issuecomment-2915053687)~ this doesn't catch the issue/[fail on the other branch](https://github.com/astral-sh/ruff/pull/18347)

---

_@thejchap reviewed on 2025-06-07 03:09_

---

_Review comment by @thejchap on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1790 on 2025-06-07 03:09_

~this should address the datadog regression~ doesn't seem to

---

_Label `ty` added by @AlexWaygood on 2025-06-07 07:57_

---

_Review comment by @thejchap on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1784 on 2025-06-12 01:13_

@sharkdp 

making slow progress on these - limited time :)

i'm still unclear on why the warning in beartype (`warning[possibly-unbound-attribute] beartype/_util/api/external/utilclick.py:108:5: Attribute `callback` on type `BeartypeableT` is possibly unbound`) is no longer being emitted on https://github.com/astral-sh/ruff/pull/18347 

however while investigating i did find what i think is an issue on `main` with setting attributes on generics where the upper bound is a union, after narrowing. added a [test](https://github.com/astral-sh/ruff/pull/18527/files#diff-19ac4a4d9337821d0bbfb544ab4e3fda87df59c6ea06065b77d3fcf5cf682ec2R1750) to illustrate

this results in a `possibly-unbound-attribute` from `ty` but no diagnostic from `mypy`

# mypy
```
../test.py:16: note: Revealed type is "test.B"
Success: no issues found in 1 source file
```

# ty
```
info[revealed-type]: Revealed type
  --> /Users/justinchapman/src/test.py:16:23
   |
14 |     if not isinstance(b, B):
15 |         raise TypeError("Expected instance of B")
16 |     print(reveal_type(b))
   |                       ^ `C & B`
17 |     b.x = 42
   |
```

# code
```py
from typing import Union, TypeVar
from typing_extensions import reveal_type

class A:
    pass

class B:
    def __init__(self) -> None:
        self.x: int = 0

C = TypeVar("C", bound=Union[A, B])

def _(b: C):
    if not isinstance(b, B):
        raise TypeError("Expected instance of B")
    print(reveal_type(b))
    b.x = 42

b = B()
_(b)
```

---

_@thejchap reviewed on 2025-06-12 01:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1784 on 2025-06-12 12:51_

That looks like a bug, yes. A similar bug was mentioned on Discord the other day. Would you mind reporting that as a separate issue? A simplified example would be https://play.ty.dev/62d91bdb-cbf1-4758-8f2c-6a26722c9f47

---

_@sharkdp reviewed on 2025-06-12 12:51_

---

_Review comment by @thejchap on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1784 on 2025-06-13 23:07_

@sharkdp https://github.com/astral-sh/ty/issues/653

---

_@thejchap reviewed on 2025-06-13 23:07_

---

_Closed by @thejchap on 2025-07-20 23:53_

---
