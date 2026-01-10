```yaml
number: 11448
title: "Used submodule import flagged as unused in `except` handler"
type: issue
state: open
author: bnomis
labels:
  - bug
assignees: []
created_at: 2024-05-16T13:05:30Z
updated_at: 2024-05-17T14:27:16Z
url: https://github.com/astral-sh/ruff/issues/11448
synced_at: 2026-01-10T11:09:53Z
```

# Used submodule import flagged as unused in `except` handler

---

_Issue opened by @bnomis on 2024-05-16 13:05_

Ruff version 0.4.4

Flags `import some.module` in the below as unused F401 but it is used

https://play.ruff.rs/f84ea9b2-0401-491f-a6a9-20f4e370d47f

```python
import logging

import some.module  # ruff thinks this is not used


def func():
    logger = logging.getLogger('some')
    try:
        import some.other
        print(some.other.version)
        return None
    except Exception as e:
        logger.exception('exception' % e, extra={'now': some.module.now()})
        raise
```


If I move the import into the function - it's no longer flagged:

```python
import logging


def func():
    import some.module  # now it's not flagged

    logger = logging.getLogger('some')
    try:
        import some.other
        print(some.other.version)
        return None
    except Exception as e:
        logger.exception('exception' % e, extra={'now': some.module.now()})
        raise
```


---

_Label `bug` added by @charliermarsh on 2024-05-17 14:25_

---

_Comment by @charliermarsh on 2024-05-17 14:26_

This seems like a mix of some issues with modeling submodule imports (we roughly treat `import some.module` and `import some.other` as equivalent) and control flow analysis.

There's more on submodule imports in (e.g.) #4656.

---

_Comment by @charliermarsh on 2024-05-17 14:27_

The interesting thing about submodule imports is -- this on its own will often:

```python
def func():
    logger = logging.getLogger('some')
    try:
        import some.other
        print(some.other.version)
        return None
    except Exception as e:
        logger.exception('exception' % e, extra={'now': some.module.now()})
        raise
```

---

_Renamed from "Used import flagged as unused F401" to "Used submodule import flagged as unused in `except` handler" by @charliermarsh on 2024-05-17 14:27_

---
