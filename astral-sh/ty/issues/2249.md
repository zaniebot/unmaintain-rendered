---
number: 2249
title: Go-to-definition jumps to typeshed stubs for datetime module import 
type: issue
state: open
author: olzhasar
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-28T16:17:16Z
updated_at: 2025-12-31T15:20:26Z
url: https://github.com/astral-sh/ty/issues/2249
synced_at: 2026-01-10T01:51:14Z
---

# Go-to-definition jumps to typeshed stubs for datetime module import 

---

_Issue opened by @olzhasar on 2025-12-28 16:17_

### Summary

When jumping to a definition ~~that corresponds to an external library~~, `ty` points to the cached Python file. This file is generally identical to the source, so exploring it's contents works fine.
However, it gets inconvenient when one wants to, for example, explore other files in the same folder as the source. This feels like a common use case when jumping to definitions.

### Version

ty 0.0.7

---

_Label `server` added by @AlexWaygood on 2025-12-28 16:33_

---

_Comment by @MichaReiser on 2025-12-28 17:01_

Can you share an example? I'm not entirely sure if I understand what you mean by cache.

The LSP also differentiates between go to definition and go to declaration: *Go to declaration* jumps to the implementation (py file) and not the definition (pyi file)

---

_Label `needs-info` added by @MichaReiser on 2025-12-28 17:01_

---

_Comment by @olzhasar on 2025-12-28 22:21_

```
from datetime import datetime
```
When I try to go to `datetime`'s definition (not type definition) using `ty` LSP here, I'm being dropped to:

`~/.cache/ty/vendored/typeshed/3c2dbb1fde8e8d1d59b10161c8bf5fd06c0011cd/stdlib/datetime.pyi`

`pyright`, on the other hand, points to the correct module in the standard library.

I thought this was happening for external imports, but it seems that it's Python's standard library imports that are causing issues.

---

_Comment by @MichaReiser on 2025-12-29 07:51_

Thanks for the example. 

ty correctly jumps to `datetime.py` when clicking on the name after `from` but we always jump to the `pyi` file when clicking on `datetime` after the `import` keyword.

I'm not too familiar with that part of the code but what I noticed:

* Once in `datetime.py`: Go to def for the `_datetime` import doesn't work. 
* `datetime.py` does use star imports. Does our definition mapper support star imports?

Either way, we should look into this. Thanks for the report

CC: @Gankra 


---

_Label `needs-info` removed by @MichaReiser on 2025-12-29 07:51_

---

_Label `bug` added by @MichaReiser on 2025-12-29 07:51_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-29 07:51_

---

_Comment by @AlexWaygood on 2025-12-29 08:51_

> * Once in `datetime.py`: Go to def for the `_datetime` import doesn't work.

I think this part, at least, is expected with our current capabilities because `_datetime` is a C extension

---

_Comment by @MichaReiser on 2025-12-29 09:04_

Makes sense. I'm not sure then how Pylance resolves `datetime` to `_pydatetime.py`

---

_Comment by @MichaReiser on 2025-12-29 09:04_

Oh, Pylance emulates:


```py
try:
    from _datetime import *
    from _datetime import __doc__
except ImportError:
    from _pydatetime import *
    from _pydatetime import __doc__
```

That's interesting

---

_Renamed from "Point to the actual file when going to the definition instead of cache" to "Go-to-definition jumps to typeshed stubs for datetime module import " by @AlexWaygood on 2025-12-29 09:05_

---

_Comment by @MichaReiser on 2025-12-29 09:06_

This is related to [#2096](https://github.com/astral-sh/ty/issues/2096)

---

_Removed from milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:20_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:20_

---
