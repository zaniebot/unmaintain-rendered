```yaml
number: 1115
title: "Ty does not remove `None` from return type after assertion"
type: issue
state: closed
author: tamir-bahar
labels: []
assignees: []
created_at: 2025-09-02T12:18:48Z
updated_at: 2025-09-02T12:32:41Z
url: https://github.com/astral-sh/ty/issues/1115
synced_at: 2026-01-10T02:06:25Z
```

# Ty does not remove `None` from return type after assertion

---

_Issue opened by @tamir-bahar on 2025-09-02 12:18_

### Summary

When analyzing the following code:

```python
from typing import reveal_type

def non_optional[T](value: T|None)->T:
    assert value is not None
    return value

def f(x: int|None):
    reveal_type(x)
    y = non_optional(x)
    reveal_type(y)
```

I expect the revealed type of `y` to be `int`, but I get `int | None`.
This behaviour is different from both mypy and pyrefly.

[ty playground link](https://play.ty.dev/962f0498-87ec-487c-8d62-8290a976815b)
[mypy playground link](https://mypy-play.net/?mypy=latest&python=3.12&gist=3bff42cf3dbcbe73e56c2a180f68b27a)
[pyrefly playground link](https://pyrefly.org/sandbox/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFAAmhwUKYKVYCMSHlAbQAqAXQAUxSgFdCALihCAPgDkOhAJQBaAHxDZ9KIahkAzicJ4okijMwm2YfKpQMjBQjCkgUV6QyYsUMBiAB7yqDAqaur6bkSklDSIhKHqBkZwUAC8DpzcvPwUqemG8eTUtClw6kA&version=3.12)

### Version

bbfcf6e11 (from the web playground)

---

_Comment by @AlexWaygood on 2025-09-02 12:32_

Thanks for the report! This should be fixed soon by some planned improvements to our constraints solver, that we're tracking in #623 (this is almost exactly the same as the example at the top of that issue, as it happens!)

---

_Closed by @AlexWaygood on 2025-09-02 12:32_

---
