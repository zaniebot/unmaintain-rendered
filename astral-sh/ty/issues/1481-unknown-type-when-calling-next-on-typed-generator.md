```yaml
number: 1481
title: "Unknown type when calling `next` on typed Generator"
type: issue
state: closed
author: biesnecker
labels:
  - generics
assignees: []
created_at: 2025-11-05T14:49:32Z
updated_at: 2025-11-05T17:45:04Z
url: https://github.com/astral-sh/ty/issues/1481
synced_at: 2026-01-10T02:06:25Z
```

# Unknown type when calling `next` on typed Generator

---

_Issue opened by @biesnecker on 2025-11-05 14:49_

### Summary

Minimal example:

```python
from collections.abc import Generator


def gen() -> Generator[int]:
    x = 0
    while True:
        yield x
        x += 1


x = gen()
y = next(x)
```

`y` is typed as `Unknown` and should be `int`.

<img width="294" height="223" alt="Image" src="https://github.com/user-attachments/assets/cc8711de-e145-44aa-b1f2-caaad531806d" />

### Version

ty 0.0.1-alpha.25 (3abd4c968 2025-10-29)

---

_Label `generics` added by @sharkdp on 2025-11-05 15:03_

---

_Comment by @sharkdp on 2025-11-05 15:04_

Thank you for reporting this. Our generics solver is currently not able to solve for `_T` here: https://github.com/python/typeshed/blob/07b69ab9e92dbe4ca55c48b1c23ebf34b41538f1/stdlib/builtins.pyi#L1699. This should soon be fixed with #623.

---

_Closed by @sharkdp on 2025-11-05 15:04_

---

_Comment by @biesnecker on 2025-11-05 15:21_

Oh, sorry for the dupe. I searched but I think I was too specific and didn't find the more general underlying issue!

---

_Comment by @sharkdp on 2025-11-05 16:39_

Of course, no worries!

---

_Comment by @AlexWaygood on 2025-11-05 17:45_

I wrote up a minimized version of this issue in https://github.com/astral-sh/ty/issues/623#issuecomment-3492551320

---
