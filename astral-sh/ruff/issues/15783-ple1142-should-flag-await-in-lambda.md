```yaml
number: 15783
title: PLE1142 should flag await in lambda
type: issue
state: closed
author: jakkdl
labels:
  - bug
assignees: []
created_at: 2025-01-28T12:10:55Z
updated_at: 2025-02-02T19:44:13Z
url: https://github.com/astral-sh/ruff/issues/15783
synced_at: 2026-01-10T11:09:57Z
```

# PLE1142 should flag await in lambda

---

_Issue opened by @jakkdl on 2025-01-28 12:10_

### Description

```python
"""silence D100"""
async def afoo() -> None:
    """silence D103"""
    return lambda: await afoo()
```
```
$ ruff check --select=ALL foo.py
[...]
All checks passed!
```
```
$ python foo.py
  File "/tmp/ruff/foo.py", line 6
    return lambda: await afoo()
                   ^^^^^^^^^^^^
SyntaxError: 'await' outside async function
```
```
$ ruff --version
ruff 0.9.3
```

---

_Comment by @jakkdl on 2025-01-28 12:13_

Upstream pylint does catch this:
```
$ pylint --disable=C0104 foo.py
************* Module foo
foo.py:4:19: E1142: 'await' should be used within an async function (await-outside-async)
```

---

_Label `bug` added by @AlexWaygood on 2025-01-28 12:35_

---

_Comment by @InSyncWithFoo on 2025-01-28 16:52_

This seems to be related to #11934.

---

_Comment by @jakkdl on 2025-01-29 10:42_

I also reported a similar issue a few months back with #14167

---

_Comment by @MichaReiser on 2025-02-02 19:44_

Thanks @InSyncWithFoo. Yes, we want to tackle this as part of #11934 

---

_Closed by @MichaReiser on 2025-02-02 19:44_

---
