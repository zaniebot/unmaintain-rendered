```yaml
number: 1853
title: "`iter` and `next` return types are unknown"
type: issue
state: closed
author: oriori1703
labels: []
assignees: []
created_at: 2025-12-11T11:25:44Z
updated_at: 2025-12-11T12:34:31Z
url: https://github.com/astral-sh/ty/issues/1853
synced_at: 2026-01-10T01:56:41Z
```

# `iter` and `next` return types are unknown

---

_Issue opened by @oriori1703 on 2025-12-11 11:25_

### Summary

It seems like `iter` and `next` builtin function always return `Unknown`.

At the beginning I thought it was an issue with `Iterable`s  but it seems to not affect `__iter__`.
This makes me this this is an issue with `SupportsIter` and `SupportsNext` from the typeshed.

my test case:
```python
from typing import reveal_type

a: list[int] = [1, 2, 3]

reveal_type(iter(a))  # expected `Iterator[int]` but got `Unknown`

b = a.__iter__() # This actually works as expected!
reveal_type(next(b))  # expected `int` but got `Unknown`
```

I was also able to reproduce the issue in the playground:
https://play.ty.dev/5431a82a-bedd-43c3-93f5-fd8704f01a99

### Version

ty 0.0.1-alpha.33

---

_Comment by @sharkdp on 2025-12-11 12:27_

Thank you for reporting this.

This is related to #1714 

---

_Closed by @sharkdp on 2025-12-11 12:27_

---

_Comment by @oriori1703 on 2025-12-11 12:34_

Makes sense.

Thanks for the quick response :)

---
