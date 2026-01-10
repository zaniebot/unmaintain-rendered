```yaml
number: 7672
title: Possible error in PLE0605
type: issue
state: closed
author: nstarman
labels:
  - bug
assignees: []
created_at: 2023-09-26T21:39:39Z
updated_at: 2023-09-27T04:36:57Z
url: https://github.com/astral-sh/ruff/issues/7672
synced_at: 2026-01-10T11:09:49Z
```

# Possible error in PLE0605

---

_Issue opened by @nstarman on 2023-09-26 21:39_

In https://github.com/astropy/astropy/pull/15385 we are trying to fix some ``__all__`` statements and ran into the following

```python
__all__ = [...]
logging_levels = ["NOTSET", ...]
...
__all__ += logging_levels
```

My suggested fix was 

```python
__all__ = [...]
__all__ += (logging_levels := ["NOTSET", ...])
```

However PLE0605 is still complaining. I don't think this is correct since the above should be, from the "perspective" of `__all__` equivalent to 

```python
__all__ = [...]
__all__ += ["NOTSET", ...]
```

which passes the check.

The inclusion of the walrus operator for assigning to another variable makes the AST more complicated but should be equivalent statically to excluding the walrus.

---

_Comment by @zanieb on 2023-09-26 21:55_

Thanks for the report!

Hm... in the example:

```python
from other import a, b

__all__ = ["a"]
__all__ += (foo := ["b"])
```

pyright reports the symbol `b` as unused and displays the warning `Operation on "__all__" is not supported, so exported symbol list may be incorrect (reportUnsupportedDunderAll)`.

Although I agree this is syntactically equivalent, if it's not supported by type checkers I'm not sure we should treat it as valid.

---

_Comment by @charliermarsh on 2023-09-26 23:45_

I think it's reasonable to support if the code changes aren't too tricky :)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 04:13_

---

_Label `bug` added by @charliermarsh on 2023-09-27 04:13_

---

_Closed by @charliermarsh on 2023-09-27 04:36_

---
