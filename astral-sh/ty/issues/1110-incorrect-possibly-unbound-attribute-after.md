```yaml
number: 1110
title: Incorrect possibly-unbound-attribute after checking for None and then sys.exit
type: issue
state: closed
author: drichardson
labels: []
assignees: []
created_at: 2025-09-01T00:27:49Z
updated_at: 2025-09-01T06:47:28Z
url: https://github.com/astral-sh/ty/issues/1110
synced_at: 2026-01-12T15:54:24Z
```

# Incorrect possibly-unbound-attribute after checking for None and then sys.exit

---

_@drichardson_

### Summary

The code below tests for `is None` and then calls `sys.exit` which has a `NoReturn` return type. However, `ty` doesn't appear to honor this and instead warns about later uses with `possibly-unbound-attribute` (even though it is guaranteed to be not `None`). Strangely, if I `raise ValueError` instead of `sys.exit` `ty` does _not_ report the warning.

Playground Link: https://play.ty.dev/b95ffd2d-734b-4aef-a32b-04741bb2f742

```python
import random
import sys
from dataclasses import dataclass

@dataclass
class Foo:
    i: int

def foo_or_none() -> Foo | None:
    return random.choice([Foo(1), Foo(2), None])

def main():
    f = foo_or_none()

    if f is None:
        print("it was none")
        sys.exit()
        # raise ValueError("it was none")

    # BUG: possibly-unbound-attribute on f.i is incorrect because sys.exit is NoReturn.
    print(f"Foo is: {f.i}")

main()
```

### Version

ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Comment by @sharkdp on 2025-09-01 06:47_

Thank you for reporting this.

Please see https://github.com/astral-sh/ty/issues/1022, https://github.com/astral-sh/ty/issues/685 and https://github.com/astral-sh/ty/issues/690.

---

_Closed by @sharkdp on 2025-09-01 06:47_

---
