```yaml
number: 11854
title: "New Rule: Formatted string passed to logging module PYL-W1203"
type: issue
state: closed
author: Zerotask
labels:
  - needs-info
assignees: []
created_at: 2024-06-13T08:42:03Z
updated_at: 2024-08-01T21:16:34Z
url: https://github.com/astral-sh/ruff/issues/11854
synced_at: 2026-01-12T15:54:51Z
```

# New Rule: Formatted string passed to logging module PYL-W1203

---

_@Zerotask_

Deepsource warns you, when you use a formatted string with the [logging](https://docs.python.org/3/library/logging.html) module.

> Formatting the message manually before passing it to a logging call does unnecessary work if logging is disabled. Consider using the logging module's built-in formatting features to avoid that.

Example:
```python
msg = "test"
logger.error(f"{msg} failed")
``` 

and suggest to use it with `%`like so:
```python
msg = "test"
logger.error("%s failed", msg)
``` 

---

_Comment by @matteo-zanoni on 2024-06-13 12:51_

I'm new to working with ruff but I would like to tackle this.
It looks similar enough to PLE1205 and PLE1206 that are already implemented an look like a good starting place.
Is this new rule something that is approved? Can I start working on it?
Would appreciate any further guidance.

---

_Comment by @tdulcet on 2024-06-13 13:17_

This is already implemented as part of G001, G002 and G004. What is still needed though is an autofix, as we have hundreds of these violations and they are very tedious to fix manually.

---

_Comment by @Zerotask on 2024-06-13 13:21_

> This is already implemented as part of G001, G002 and G004. What is still needed though is an autofix, as we have hundreds of these violations and they are very tedious to fix manually.

We're using the "G" rule and ruff does not detect this.

---

_Comment by @charliermarsh on 2024-06-13 23:54_

I see it raised here: https://play.ruff.rs/4e92508d-36ee-476b-b67d-7bc6d8c1e457

---

_Label `needs-info` added by @charliermarsh on 2024-06-13 23:54_

---

_Comment by @Zerotask on 2024-06-14 08:24_

> I see it raised here: https://play.ruff.rs/4e92508d-36ee-476b-b67d-7bc6d8c1e457

It seems that the error fades, when I use for example `import logger`. In our case we create a logger instance in another module and just import it then.
If I create the logger in the same module, the error will be shown.

---

_Comment by @dhruvmanila on 2024-06-14 09:17_

> It seems that the error fades, when I use for example `import logger`. In our case we create a logger instance in another module and just import it then. If I create the logger in the same module, the error will be shown.

Yes, that's currently a limitation of Ruff where it doesn't know the context from other modules. Currently, Ruff only looks at one file at a time but that's going to be changed in the future where we add support for multifile analysis.

---

_Comment by @matteo-zanoni on 2024-06-14 13:54_

Actually it looks like those errors are either raised or not based only on the name of the object calling the `error`, `info`, etc... function.

So for example:

```py
msg = "test"

class Foo:
    def __init__(self):
        ...

    def error(self, msg: str):
        raise ValueError(msg)

log = Foo()
log.error(f"{msg} failed")
```

this raises the error but

```py
msg = "test"

class Foo:
    def __init__(self):
        ...

    def error(self, msg: str):
        raise ValueError(msg)

foo = Foo()
foo.error(f"{msg} failed")
``` 

this does not.
In this case neither should be an error (since they are not calls to the logging python module), but the same holds for instances where the error should be raised:

This shows an error

```py
import logging

msg = "test"
logger = logging.getLogger("foo")
logger.error(f"{msg} failed")
```

But this does not

```py
import logging

msg = "test"
foo = logging.getLogger("foo")
foo.error(f"{msg} failed")
```

---

_Comment by @MichaReiser on 2024-08-01 21:15_

Let's merge this with https://github.com/astral-sh/ruff/issues/12613

---

_Closed by @MichaReiser on 2024-08-01 21:15_

---
