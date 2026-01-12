```yaml
number: 2258
title: "`PT013`: Ignore imports in `TYPE_CHECKING` block?"
type: issue
state: closed
author: ngnpope
labels:
  - question
assignees: []
created_at: 2023-01-27T14:51:43Z
updated_at: 2023-05-18T16:02:55Z
url: https://github.com/astral-sh/ruff/issues/2258
synced_at: 2026-01-12T15:54:42Z
```

# `PT013`: Ignore imports in `TYPE_CHECKING` block?

---

_@ngnpope_

Consider the following:

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from pytest import MonkeyPatch

def test_something(monkeypatch: MonkeyPatch) -> None:
    ...
```

This results in the following:

```console
❯ ruff --version
ruff 0.0.236

❯ ruff --select PT013 bug.py 
bug.py:4:5: PT013 Found incorrect import of pytest, use simple `import pytest` instead
Found 1 error.
```

It seems that `PT013` is largely driven by trying to avoid things like `from pytest import fixture` and `from pytest import mark`, but it'd be pretty ugly to be forced to use `pytest.MonkeyPatch`, etc. everywhere we want to type hint things...

I guess a workaround is to do something like the following:

```python
from typing import TYPE_CHECKING, TypeAlias

import pytest

if TYPE_CHECKING:
    MonkeyPatch: TypeAlias = pytest.MonkeyPatch

def test_something(monkeypatch: MonkeyPatch) -> None:
    assert 0
```

But this feels a little unnatural... Can we make `PT013` not apply in `TYPE_CHECKING` blocks?

---

_Label `question` added by @charliermarsh on 2023-01-27 15:32_

---

_Renamed from "PT013: Ignore imports in `TYPE_CHECKING` block?" to "`PT013`: Ignore imports in `TYPE_CHECKING` block?" by @ngnpope on 2023-02-09 16:21_

---

_Comment by @charliermarsh on 2023-05-18 16:02_

I can see why this is useful, but I think it would arguably be surprising to special-case it. I'm going to close as "not planned".

---

_Closed by @charliermarsh on 2023-05-18 16:02_

---
