```yaml
number: 12873
title: "`ASYNC100` does not match upstream `flake8-async`"
type: issue
state: closed
author: Zac-HD
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-08-14T01:25:36Z
updated_at: 2024-08-15T01:02:58Z
url: https://github.com/astral-sh/ruff/issues/12873
synced_at: 2026-01-12T15:54:52Z
```

# `ASYNC100` does not match upstream `flake8-async`

---

_@Zac-HD_

`ASYNC100` ([ruff](https://docs.astral.sh/ruff/rules/cancel-scope-no-checkpoint/), [upstream](https://flake8-async.readthedocs.io/en/latest/rules.html#async100)) warns when a timeout context manager doesn't contain any checkpoints - and therefore can't be cancelled by the async framework.  However, if you're _implementing_ a context manager, then it's entirely reasonable to wrap such a timeout around a `yield`, and we should treat that as a checkpoint - as in https://github.com/python-trio/flake8-async/pull/228

(if you wrap a timeout around the `yield` in a generator, see [PEP-789](https://peps.python.org/pep-0789/) and the ASYNC9xx rules, with my condolences)

```python
from contextlib import asynccontextmanager
import anyio

async def bad_code():
    with anyio.fail_after(10):
        do_something_slow()  # can't cancel non-async work!


@asynccontextmanager
async def good_code():
    with anyio.fail_after(10):
        # There's no await keyword here, but we presume that there
        # will be in the caller we yield to, so this is safe.
        yield
```

The current version of Ruff will warn on both `fail_after()` contexts, but only the former is problematic.

See also: https://github.com/astral-sh/ruff/issues/8451

---

_Comment by @charliermarsh on 2024-08-14 01:32_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-08-14 01:32_

---

_Label `help wanted` added by @MichaReiser on 2024-08-14 06:53_

---

_Closed by @charliermarsh on 2024-08-15 01:02_

---

_Closed by @charliermarsh on 2024-08-15 01:02_

---
