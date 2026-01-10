```yaml
number: 310
title: False positives missing arg on inherited class
type: issue
state: closed
author: ion-elgreco
labels:
  - bug
  - calls
assignees: []
created_at: 2025-05-10T16:45:22Z
updated_at: 2025-05-12T10:23:16Z
url: https://github.com/astral-sh/ty/issues/310
synced_at: 2026-01-10T02:34:09Z
```

# False positives missing arg on inherited class

---

_Issue opened by @ion-elgreco on 2025-05-10 16:45_

### Summary

Example:

```python
import dagster as dg

class Foo(dg.ConfigurableResource):
    def do_something(self, first_arg: str) -> None:
        print(first_arg)
        return None

Foo().do_something("random")
```

Returns:
```python
error[missing-argument]: No argument provided for required parameter `self` of function `__init__`
  --> test.py:10:1
   |
10 | Foo().do_something("random")
   | ^^^^^
   |
info: `missing-argument` is enabled by default

error[missing-argument]: No argument provided for required parameter `first_arg` of function `do_something`
  --> test.py:10:1
   |
10 | Foo().do_something("random")
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
info: `missing-argument` is enabled by default
```

### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Label `calls` added by @MichaReiser on 2025-05-12 06:46_

---

_Label `bug` added by @MichaReiser on 2025-05-12 06:46_

---

_Comment by @sharkdp on 2025-05-12 10:23_

I think the underlying cause is the same as #312. I'm going to close this as a duplicate. Thank you for reporting it.

---

_Closed by @sharkdp on 2025-05-12 10:23_

---
