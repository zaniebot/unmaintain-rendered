```yaml
number: 1579
title: "Regression: typing.Self diagnostics"
type: issue
state: closed
author: jeffective
labels: []
assignees: []
created_at: 2025-11-18T00:07:31Z
updated_at: 2025-11-18T00:31:22Z
url: https://github.com/astral-sh/ty/issues/1579
synced_at: 2026-01-10T02:06:25Z
```

# Regression: typing.Self diagnostics

---

_Issue opened by @jeffective on 2025-11-18 00:07_

### Summary

This was working in 0.0.1-alpha.25 but regressed in 0.0.1-alpha.26

```python
from typing import Self

class Encoding:
    APPLICATION_CBOR: Self

def use_encoding(encoding: Encoding):
    pass

use_encoding(Encoding.APPLICATION_CBOR)
```

```
Argument to function `use_encoding` is incorrect: Expected `Encoding`, found `typing.Self` (invalid-argument-type) [Ln 9, Col 14]
```

Playground link: https://play.ty.dev/0e180658-87a1-4b6b-83b8-ab6a34dfd8b8


### Version

0.0.1-alpha.26

---

_Comment by @carljm on 2025-11-18 00:31_

Thanks for the minimized report!

It looks to me like this fails in the same way ty alpha.25, so I suspect that some other improvement in alpha.26 made the type visible to ty in your full codebase context?

But the root issue is #1124 

---

_Closed by @carljm on 2025-11-18 00:31_

---
