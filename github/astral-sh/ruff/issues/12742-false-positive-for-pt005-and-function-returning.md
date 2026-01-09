---
number: 12742
title: "False positive for `PT005` and function returning `Iterator[None]`"
type: issue
state: closed
author: ZeeD
labels:
  - rule
  - needs-mre
assignees: []
created_at: 2024-08-08T07:09:52Z
updated_at: 2024-08-15T17:14:16Z
url: https://github.com/astral-sh/ruff/issues/12742
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive for `PT005` and function returning `Iterator[None]`

---

_Issue opened by @ZeeD on 2024-08-08 07:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Minimal snippet:

```python
from collections.abc import Iterator

import pytest

@pytest.fixture()
def _my_fixture_none() -> None:
  return

@pytest.fixture()
def _my_fixture_itor() -> Iterator[None]:
  yield

def test_mytest(
  _my_fixture_none: None,
  _my_fixture_itor: None,
):
    ...
```

ruff returns error `PT005` on `_my_fixture_itor`. It shouldn't.

According to https://docs.pytest.org/en/latest/reference/reference.html#pytest-fixture 

> Fixtures can provide their values to test functions using return or yield statements. When using yield the code block after the yield statement is executed as teardown code regardless of the test outcome, and must yield exactly once.

So a fixture that work only by side effect may `return` `None` or `Iterator[None]` - and in fact if you use a fixture that `return`s `Iterator[None]` have the variable set to `None`


---

_Label `rule` added by @charliermarsh on 2024-08-09 01:20_

---

_Label `needs-decision` added by @charliermarsh on 2024-08-09 01:20_

---

_Comment by @charliermarsh on 2024-08-09 01:21_

Is the suggestion that both implicit `return` and implicit `yield` (without a value) should be allowed?

---

_Comment by @charliermarsh on 2024-08-09 01:22_

I don't get a PT005 error on this: https://play.ruff.rs/b404e34f-2c6e-4228-a300-cd783adb9980. Can you help me reproduce?

---

_Label `needs-decision` removed by @charliermarsh on 2024-08-09 01:22_

---

_Label `needs-mre` added by @charliermarsh on 2024-08-09 01:23_

---

_Comment by @ZeeD on 2024-08-09 07:25_

Sorry, it seems there was a fluke from my side.

I got the `PT005` because in my actual code I have explicit `yield None`
In fact I see that I can see the `PT005` if I write

```python
@pytest.fixture
def _my_fixture_none() -> None:
    return None

@pytest.fixture
def _my_fixture_itor() -> Iterator[None]:
    try:
        yield None
    finally:
        ...
```

cfr: https://play.ruff.rs/cd4496f2-4176-4a30-8471-8dee0b8d090a


regarding this gh issue, I understand that the explicit `None` is not necessary, but should be "functionally equivalent" of the bare `return` and `yield`, and it seems a bit strange to me how this is related to the `PT005` itself. In any case I can see that the error was in my code, not in ruff.

Maybe it could be possible to better rephrase the error message / add a note in the documentation page?


---

_Comment by @MichaReiser on 2024-08-15 17:14_

Thanks for the example! 

We decided to deprecate `PT005` because it has other flaws and isn't recommended by the community. I close the issue because we don't plan on making improvements to the rule further.

---

_Closed by @MichaReiser on 2024-08-15 17:14_

---
