```yaml
number: 2562
title: "flake8-logging-format should check `.log(level, msg)` calls too"
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2023-02-04T00:59:12Z
updated_at: 2023-03-25T15:55:55Z
url: https://github.com/astral-sh/ruff/issues/2562
synced_at: 2026-01-10T11:09:45Z
```

# flake8-logging-format should check `.log(level, msg)` calls too

---

_Issue opened by @andersk on 2023-02-04 00:59_

```python
import logging
logger = logging.getLogger("test")
a, b = "a", "b"
logger.error(f"{a} {b}")  # raises G004 Logging statement uses f-string
logger.log(logging.ERROR, f"{a} {b}")  # should raise G004, but doesn’t
```

Also reported upstream at globality-corp/flake8-logging-format#69.

---

_Comment by @charliermarsh on 2023-02-04 01:29_

Can do!

---

_Label `bug` added by @charliermarsh on 2023-02-04 01:31_

---

_Comment by @dhruvmanila on 2023-03-24 18:25_

So, I'm looking into this and have a few queries:

As per the [documentation of the `log` function](https://docs.python.org/3/library/logging.html#logging.Logger.log), it says that the `level` argument could be any integer level. In addition to that, it allows [adding](https://docs.python.org/3/library/logging.html#logging.addLevelName) of custom log levels as well. The user could pass in any arbitrary integer as well and that works.

_Should we support any arbitrary integer levels or only the ones defined in the standard library?_

This is more towards the implementation side: I see that the implementation has been grouped together instead of each rule being a separate module. With the support of `logging.log`, special handling would be required where it's either one or the other. Are there any plans to refactor into separate modules or is this ok for now?

---

_Comment by @andersk on 2023-03-24 19:07_

We have no need to validate or even inspect the `level` argument. (It’s probably going to be a variable anyway.) We just need to apply the same string formatting rules to `logger.log(level, msg)` that we would apply to `logger.debug(msg)`.

---

_Closed by @charliermarsh on 2023-03-25 15:55_

---
