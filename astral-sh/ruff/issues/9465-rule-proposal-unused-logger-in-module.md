```yaml
number: 9465
title: "Rule Proposal: unused logger in module"
type: issue
state: open
author: valentincalomme
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-01-11T10:08:50Z
updated_at: 2024-01-13T09:57:57Z
url: https://github.com/astral-sh/ruff/issues/9465
synced_at: 2026-01-10T11:09:51Z
```

# Rule Proposal: unused logger in module

---

_Issue opened by @valentincalomme on 2024-01-11 10:08_

# unused-logger (RUF-0XX)

## What it does

Checks if a logger isn't used in the module it's instantiated.

## Why is it bad

Although you could instantiate a logger in one file and use it in another, this is not recommended. Therefore, if a logger isn't used in the module, it's instantiated; it should be considered unused and removed.

## Example

If I have a simple `module.py`

```py
"""A simple module."""
import logging

logger = logging.getLogger(__name__)


def foo() -> None:
    ...
```

The logger isn't used, which adds 2 unnecessary lines. The linter should fix the file so that it looks like this:

```py
"""A simple module."""


def foo() -> None:
    ...

```

## When not to use it

If you don't care about the additional lines.

---

_Label `rule` added by @charliermarsh on 2024-01-13 00:51_

---

_Label `needs-decision` added by @charliermarsh on 2024-01-13 00:51_

---

_Comment by @charliermarsh on 2024-01-13 00:52_

I thought it was somewhat common to define a logger and import it elsewhere -- is that not recommended?

---

_Comment by @zanieb on 2024-01-13 01:06_

Is `logger` excluded from unused variable rules?

> I thought it was somewhat common to define a logger and import it elsewhere -- is that not recommended?

It is common, but not recommended by the official docs. They're clear that they recommend a new logger per module with `__name__`.

---

_Comment by @charliermarsh on 2024-01-13 01:10_

Variables defined at the top-level of the module are always excluded from unused variable rules.

---

_Comment by @valentincalomme on 2024-01-13 09:57_

> It is common, but not recommended by the official docs. They're clear that they recommend a new logger per module with __name__.

This is what I'm trying to enforce basically. It's an edge case specifically for any variable at the top of the module that is assigned the value `logging.getLogger(__name__)`.

I think it could be this specific as `ruff` already has other rules to enforce how to instantiate a logger (i.e., LOG0XX rules)

---
