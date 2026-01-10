```yaml
number: 1649
title: "Too many arguments for `type[tuple[Any, ...]]`"
type: issue
state: closed
author: metaist
labels:
  - typing semantics
assignees: []
created_at: 2025-11-27T00:28:31Z
updated_at: 2025-12-01T10:49:28Z
url: https://github.com/astral-sh/ty/issues/1649
synced_at: 2026-01-10T01:58:59Z
```

# Too many arguments for `type[tuple[Any, ...]]`

---

_Issue opened by @metaist on 2025-11-27 00:28_

### Summary

https://play.ty.dev/76cd71f0-4755-4309-8277-2ee2740494d6

`tuple[Any, ...]` works fine, but when it's `type[tuple[Any, ...]]` the variadic tuple signature is rejected.

```python
from typing import Any
ok: tuple[Any, ...]
fails: type[tuple[Any, ...]] # too-many-positional-arguments
```
```
error[too-many-positional-arguments]: Too many positional arguments to class `tuple`: expected 1, got 2
 --> test_variadic_tuple_type.py:4:13
  |
3 | ok: tuple[Any, ...]
4 | fails: type[tuple[str, ...]]
  |             ^^^^^^^^^^^^^^^
  |
info: rule `too-many-positional-arguments` is enabled by default
```

### Version

ty 0.0.1-alpha.28

---

_Comment by @carljm on 2025-11-27 00:40_

Nice timing, I literally just ran into this bug a minute ago myself :) Thanks for the report!

---

_Label `typing semantics` added by @carljm on 2025-11-27 00:40_

---

_Added to milestone `Stable` by @carljm on 2025-11-27 00:41_

---

_Closed by @sharkdp on 2025-12-01 10:49_

---
