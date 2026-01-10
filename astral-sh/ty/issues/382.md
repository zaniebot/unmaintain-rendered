```yaml
number: 382
title: "3rd party modules with the same namespace as 1st party code aren't being discovered"
type: issue
state: closed
author: mrlifetime
labels:
  - imports
assignees: []
created_at: 2025-05-14T11:20:38Z
updated_at: 2025-05-14T11:42:24Z
url: https://github.com/astral-sh/ty/issues/382
synced_at: 2026-01-10T02:34:09Z
```

# 3rd party modules with the same namespace as 1st party code aren't being discovered

---

_Issue opened by @mrlifetime on 2025-05-14 11:20_

### Summary

Hi, I have a repo with a structure like this:

```
root / mynamespace / mymodule / __init__.py
```

`mynamespace` is a subdir that doesn't contain anything, it is a [namespace](https://packaging.python.org/en/latest/guides/packaging-namespace-packages/).
I am importing other 3rd party modules (installed via pip) that share the same namespace, e.g. `from mynamespace import myothermodule`.

In my root I have the `main.py` file that imports both:

```py
import mynamespace.mymodule # ty discovers this fine (1st party)
import mynamespace.myothermodule # error! Cannone resolve imported module `mynamespace.myothermodule`
```

Please let me know if you require additional logs or info.

### Version

ty 0.0.0-alpha.7 on Linux

---

_Renamed from "3rd party modules with the same namespace aren't being discovered" to "3rd party modules with the same namespace as 1st party code aren't being discovered" by @mrlifetime on 2025-05-14 11:22_

---

_Comment by @mrlifetime on 2025-05-14 11:27_

This is a very likely dup of https://github.com/astral-sh/ty/issues/363 and https://github.com/astral-sh/ty/issues/375 feel free to close if you agree.

---

_Closed by @mrlifetime on 2025-05-14 11:27_

---

_Label `imports` added by @AlexWaygood on 2025-05-14 11:42_

---
