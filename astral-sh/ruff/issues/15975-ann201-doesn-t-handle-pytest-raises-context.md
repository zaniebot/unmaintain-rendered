```yaml
number: 15975
title: "ANN201 doesn't handle pytest.raises context manager"
type: issue
state: open
author: jogo-openai
labels:
  - bug
assignees: []
created_at: 2025-02-05T17:43:32Z
updated_at: 2025-02-06T08:16:53Z
url: https://github.com/astral-sh/ruff/issues/15975
synced_at: 2026-01-12T15:54:55Z
```

# ANN201 doesn't handle pytest.raises context manager

---

_@jogo-openai_

### Description

ruff version: 0.9.4

code:
```python
import pytest


def test_exception():
    with pytest.raises(ValueError):
        raise ValueError("This is a test exception")
    a = 1
    assert a == 1


def a_function():
    return 1


def no_return():
    pass
```

`ruff check --select=ANN201 --diff --unsafe-fixes  example.py`
```diff
--- example.py
+++ example.py
@@ -1,16 +1,17 @@
 import pytest
+from typing import NoReturn


-def test_exception():
+def test_exception() -> NoReturn:
     with pytest.raises(ValueError):
         raise ValueError("This is a test exception")
     a = 1
     assert a == 1


-def a_function():
+def a_function() -> int:
     return 1


-def no_return():
+def no_return() -> None:
     pass

Would fix 3 errors.
```

After applying the fix mypy reports the error:

```~ mypy example.py
example.py:5: error: Implicit return in function which does not return  [misc]
```

---

_Label `bug` added by @MichaReiser on 2025-02-06 08:13_

---

_Comment by @MichaReiser on 2025-02-06 08:16_

Thanks for the report. The issue is really only about 

```py
def test_exception():
    with pytest.raises(ValueError):
        raise ValueError("This is a test exception")
    a = 1
    assert a == 1
```

specifically that Ruff adds the return type `Never` which is incorrect because `pytest.raises` catches the exception and doesn't propagate it. 


Since it is hard for ruff to know if a context manager handles an exception (because it can't resolve the context manager nor its methods if defined in another file). We have to explore whether we can provide any type suggestion in that case (by handling a context manager similar to `try`) or if we need to bail out of providing a fix.

Relevant code: https://github.com/astral-sh/ruff/blob/13ffb5bc1936493816f01fb5583359e3d8fffca7/crates/ruff_python_semantic/src/analyze/terminal.rs#L42-L150

---
