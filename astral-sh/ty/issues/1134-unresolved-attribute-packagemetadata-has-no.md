```yaml
number: 1134
title: "unresolved-attribute: `PackageMetadata` has no attribute `get`"
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2025-09-05T14:24:05Z
updated_at: 2025-09-05T18:08:10Z
url: https://github.com/astral-sh/ty/issues/1134
synced_at: 2026-01-10T02:06:25Z
```

# unresolved-attribute: `PackageMetadata` has no attribute `get`

---

_Issue opened by @janosh on 2025-09-05 14:24_

### Summary

```py
import importlib.metadata

package = 'importlib'
metadata = importlib.metadata.metadata(package)
homepage = metadata.get("version")
>>> error[unresolved-attribute] Type `PackageMetadata` has no attribute `get`
```

[not sure where that false positive is coming from](https://github.com/python/cpython/blob/e76464d161f2e7f2c27c84f8de4de5f88f025973/Lib/importlib/metadata/_meta.py#L24-L27)

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @sharkdp on 2025-09-05 15:30_

Thank you for reporting this.

I can not reproduce this: https://play.ty.dev/84bcd802-aa50-4100-88bd-721dcb3b9b0b

---

_Comment by @sharkdp on 2025-09-05 15:31_

Probably need to configure your Python version to 3.12 or higher.

---

_Comment by @carljm on 2025-09-05 16:38_

For the standard library, type checkers look at typeshed stubs, so it's more useful to look there than in the actual stdlib code to understand type checker behavior: https://github.com/python/typeshed/blob/main/stdlib/importlib/metadata/_meta.pyi#L22-L26

Will close this as "duplicate" of https://github.com/astral-sh/ty/issues/296, just to collect reports where that diagnostic improvement would have helped in finding the problem.

---

_Closed by @carljm on 2025-09-05 16:38_

---

_Comment by @janosh on 2025-09-05 16:41_

thanks @sharkdp, good catch! setting `requires-python = ">=3.12"` in `pyproject.toml` fixes the error

---

_Comment by @sharkdp on 2025-09-05 18:08_

I meant to write a somewhat longer reply, but then I got a call and thought I'd send the short version anyway. Glad it helped.

---
