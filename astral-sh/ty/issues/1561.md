```yaml
number: 1561
title: Render PEP-257-style docstrings to markdown
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-11-14T19:06:01Z
updated_at: 2025-11-21T16:02:10Z
url: https://github.com/astral-sh/ty/issues/1561
synced_at: 2026-01-10T01:58:59Z
```

# Render PEP-257-style docstrings to markdown

---

_Issue opened by @Gankra on 2025-11-14 19:06_

https://peps.python.org/pep-0257/

This format isn't explicitly specified in the PEP but it is alluded to and used in the stdlib. For instance:

```py
def filterwarnings(action, message="", category=Warning, module="", lineno=0,
                   append=False):
    """Insert an entry into the list of warnings filters (at the front).

    'action' -- one of "error", "ignore", "always", "default", "module",
                or "once"
    'message' -- a regex that the warning message must match
    'category' -- a class that the warning must be a subclass of
    'module' -- a regex that the module name must match
    'lineno' -- an integer line number, 0 matches all warnings
    'append' -- if true, append to the list of filters
    """
```


---

_Label `server` added by @Gankra on 2025-11-14 19:06_

---

_Added to milestone `Stable` by @Gankra on 2025-11-14 19:06_

---

_Comment by @Gankra on 2025-11-21 16:02_

MVP implemented by https://github.com/astral-sh/ruff/pull/21550

---

_Closed by @Gankra on 2025-11-21 16:02_

---
