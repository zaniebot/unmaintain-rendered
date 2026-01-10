---
number: 1741
title: ARG002 and Protocols
type: issue
state: closed
author: nstarman
labels: []
assignees: []
created_at: 2023-01-09T04:59:33Z
updated_at: 2023-01-09T05:36:17Z
url: https://github.com/astral-sh/ruff/issues/1741
synced_at: 2026-01-10T01:22:39Z
---

# ARG002 and Protocols

---

_Issue opened by @nstarman on 2023-01-09 04:59_

ARG002 is raised for `typing.Protocol` classes, which generally do not have an implementation.
I don't think these should be reported, or at suppressed by a configuration option.

```python

class Example(Protocol):
    def method(self, arg1: object) -> object:
        ...
```

---

_Comment by @charliermarsh on 2023-01-09 05:10_

The above example actually doesn't trigger `ARG002` for me:

```py
class Example(Protocol):
    def method(self, arg1: object) -> object:
        ...
```

```
❯ ruff --isolated foo.py --select ARG -n
```

(We omit methods with empty bodies, so that makes sense to me -- are you setting otherwise though?)

It _does_ trigger if I include a docstring, which is probably incorrect:

```py
class Example(Protocol):
    def method(self, arg1: object) -> object:
        """Some docstring."""
        ...
```


---

_Referenced in [astral-sh/ruff#1742](../../astral-sh/ruff/pulls/1742.md) on 2023-01-09 05:24_

---

_Comment by @charliermarsh on 2023-01-09 05:24_

If that _is_ the issue, it's fixed in #1742.

---

_Closed by @charliermarsh on 2023-01-09 05:34_

---

_Comment by @charliermarsh on 2023-01-09 05:34_

(Just LMK if that's _not_ the issue and I'll re-open!)

---

_Comment by @nstarman on 2023-01-09 05:36_

That does appear to be the issue, thanks! I have docstrings in my Protocols.

---
