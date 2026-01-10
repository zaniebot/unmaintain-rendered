```yaml
number: 7610
title: PT022 doesnt correct type annotations
type: issue
state: closed
author: RonnyPfannschmidt
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-09-22T19:20:54Z
updated_at: 2023-09-24T06:39:49Z
url: https://github.com/astral-sh/ruff/issues/7610
synced_at: 2026-01-10T11:09:49Z
```

# PT022 doesnt correct type annotations

---

_Issue opened by @RonnyPfannschmidt on 2023-09-22 19:20_

given
```python
import pytest
from typing import Generator


@pytest.fixture()
def testme() -> Generator[int, None, None]:
    a = 1
    yield a
```

after running

`ruff --isolated --select PT ruff_check.py --fix`

im left with 
```python
import pytest
from typing import Generator


@pytest.fixture()
def testme() -> Generator[int, None, None]:
    a = 1
    return a
```

instead of 
```python
import pytest
from typing import Generator


@pytest.fixture()
def testme() -> int:
    a = 1
    return a
```

---

_Label `bug` added by @charliermarsh on 2023-09-22 19:36_

---

_Comment by @charliermarsh on 2023-09-22 19:36_

Thanks for filing. Just confirming, the issue is that the type annotation should be changed to reflect that the method is no longer a generator, right?

---

_Comment by @RonnyPfannschmidt on 2023-09-22 19:41_

exactly

---

_Label `autofix` added by @dhruvmanila on 2023-09-23 05:17_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-23 05:17_

---

_Closed by @dhruvmanila on 2023-09-24 06:39_

---
