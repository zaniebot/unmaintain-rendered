```yaml
number: 1303
title: "Does not treat `pytest.fail` like `raise`"
type: issue
state: closed
author: chriscarrollsmith
labels: []
assignees: []
created_at: 2025-10-03T20:48:16Z
updated_at: 2025-10-06T07:53:44Z
url: https://github.com/astral-sh/ty/issues/1303
synced_at: 2026-01-12T15:54:24Z
```

# Does not treat `pytest.fail` like `raise`

---

_@chriscarrollsmith_

### Summary

`ty` doesn't understand that after pytest.fail(), execution stops, so it still thinks `id` could be `None`:

```python
import pytest
import random


@pytest.fixture
def id() -> int | None:
    return random.choice([42, None])


def do_something_with_id(id: int) -> None:
    print(f"Processing id: {id}")


def test_do_something_with_id(id: int | None) -> None:
    if id is None:
        pytest.fail("id cannot be None")
    do_something_with_id(id)  # Type checker error: id could still be None
```

Output:

```console
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                  error[invalid-argument-type]: Argument to function `do_something_with_id` is incorrect
  --> tests/test_example.py:17:26
   |
15 |     if id is None:
16 |         pytest.fail("id cannot be None")
17 |     do_something_with_id(id)  # Type checker error: id could still be None
   |                          ^^ Expected `int`, found `int | None`
   |
info: Function defined here
  --> tests/test_example.py:10:5
   |
10 | def do_something_with_id(id: int) -> None:
   |     ^^^^^^^^^^^^^^^^^^^^ ------- Parameter declared here
11 |     print(f"Processing id: {id}")
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1a21

---

_Comment by @sharkdp on 2025-10-06 07:53_

Thank you for reporting this.

We already support unreachability-detection when calling functions (callables, in this case) that return `NoReturn` in control flow analysis, but we do not yet preserve narrowing constraints in these cases.

This is already tracked in #690.

---

_Closed by @sharkdp on 2025-10-06 07:53_

---
