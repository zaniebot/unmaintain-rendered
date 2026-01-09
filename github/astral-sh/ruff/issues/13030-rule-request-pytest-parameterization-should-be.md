---
number: 13030
title: "Rule request: pytest parameterization should be consistent"
type: issue
state: open
author: bentheiii
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-08-21T10:18:39Z
updated_at: 2024-08-29T06:52:10Z
url: https://github.com/astral-sh/ruff/issues/13030
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule request: pytest parameterization should be consistent

---

_Issue opened by @bentheiii on 2024-08-21 10:18_

Hi, I'd like to propose a new rule for pytest, wherein the order of parameterized tests must be consistent within runs. By that I mean that the following should trigger the rule
```py
import pytest

@pytest.mark.parametrize("x", {1,2})  # <-- error
def test_foo(x: int):
    ...
```

The reason for this is plugins like https://github.com/mark-adams/pytest-test-groups (and its various forks) that require that tests be ordered.

A possible way to implement this is to check that the parameter values is either a list or tuple literal, or is wrapped by a `sorted` call

```py
import pytest

# say we want to test against all non-square numbers up to 100

NUMBERS = set(range(100))
SQUARES = { i**2 for i in range(10) }

@pytest.mark.parametrize("x", NUMBERS - SQUARES)  # <-- will trigger the check
def test_foo(x: int):
    ...

@pytest.mark.parametrize("x", sorted(NUMBERS - SQUARES))  # <-- OK
def test_bar(x: int):
    ...
```

---

_Label `rule` added by @MichaReiser on 2024-08-22 13:19_

---

_Label `needs-decision` added by @MichaReiser on 2024-08-22 13:19_

---

_Comment by @MichaReiser on 2024-08-22 13:21_

I can see how such a rule is desired for users of `pytest-test-groups` but I'm worried that it would also flag unsorted `pytest.mark.parametrize` usages for everyone not using `pytest-test-groups` and they would have to disable the rule manually. 

We also try to avoid adding framework-specific rules unless the framework is widely adopted in the community (a community standard). 

---

_Comment by @bentheiii on 2024-08-22 13:59_

> We also try to avoid adding framework-specific rules unless the framework is widely adopted in the community (a community standard).

I'd argue that consistently ordered tests (though not necessarily consistently-executed) tests are an accepted best-practice standard of pytest. If users want tests in random order, that should be made explicit with specific plugins like pytest-randomly.

As for the false-positive, I can't think of a way around that except for creating a new category for this rule. If that seems like overkill, then I assume this issue can be closed.

---

_Comment by @scastlara on 2024-08-26 18:40_

For what is worth, [pytest-xdist](https://github.com/pytest-dev/pytest-xdist) also requires test cases in a parametrize mark to have a consistent order: https://pytest-xdist.readthedocs.io/en/stable/known-limitations.html#order-and-amount-of-test-must-be-consistent 

---

_Comment by @bentheiii on 2024-08-29 06:52_

> For what is worth, [pytest-xdist](https://github.com/pytest-dev/pytest-xdist) also requires test cases in a parametrize mark to have a consistent order: https://pytest-xdist.readthedocs.io/en/stable/known-limitations.html#order-and-amount-of-test-must-be-consistent

I didn't know that. I'd argue that xdist is widely enough used to warrant a rule like this

---
