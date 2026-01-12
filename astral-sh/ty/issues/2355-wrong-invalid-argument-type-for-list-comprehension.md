```yaml
number: 2355
title: Wrong invalid-argument-type for list comprehension
type: issue
state: closed
author: wardVD
labels:
  - bidirectional inference
assignees: []
created_at: 2026-01-06T08:05:00Z
updated_at: 2026-01-06T16:12:21Z
url: https://github.com/astral-sh/ty/issues/2355
synced_at: 2026-01-12T15:54:26Z
```

# Wrong invalid-argument-type for list comprehension

---

_@wardVD_

### Summary

We get an [invalid-argument-type](https://ty.dev/rules#invalid-argument-type) for the following code:

```python
from typing import Literal

def test(test_string: Literal["a", "b"]) -> str:
    return test_string

[test(string) for string in ["a", "b"]]
```

The `string` variable returns

```
Argument to function `test` is incorrect: Expected `Literal["a", "b"]`, found `Unknown | str` ty[invalid-argument-type](https://ty.dev/rules#invalid-argument-type)
```

From the code it should be clear that the parameters can only be `"a"` or `"b"` so I believe this is a bug.

### Version

uv 0.9.21 (Homebrew 2025-12-30)

---

_Label `bidirectional inference` added by @AlexWaygood on 2026-01-06 10:05_

---

_Comment by @ibraheemdev on 2026-01-06 16:12_

Thanks for the report! This is a known bug, we don't yet support bidirectional inference with list comprehensions, and so aren't able to avoid literal promotion.

---

_Closed by @ibraheemdev on 2026-01-06 16:12_

---
