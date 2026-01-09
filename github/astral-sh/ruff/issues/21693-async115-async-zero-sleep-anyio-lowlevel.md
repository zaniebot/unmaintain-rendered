---
number: 21693
title: "ASYNC115 (async-zero-sleep): `anyio.lowlevel.checkpoint()` needs `import anyio.lowlevel`"
type: issue
state: open
author: injust
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-11-29T10:40:15Z
updated_at: 2025-12-08T01:41:10Z
url: https://github.com/astral-sh/ruff/issues/21693
synced_at: 2026-01-07T13:12:16-06:00
---

# ASYNC115 (async-zero-sleep): `anyio.lowlevel.checkpoint()` needs `import anyio.lowlevel`

---

_Issue opened by @injust on 2025-11-29 10:40_

### Summary

```python
import anyio

async def test() -> None:
    await anyio.sleep(0)
```

https://play.ruff.rs/86a8f945-5295-4092-ae43-81de88b90a63

ASYNC115 auto-fixes this to:

```python
import anyio

async def test() -> None:
    await anyio.lowlevel.checkpoint()
```

This doesn't type check because `anyio.lowlevel` isn't imported.

### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-11-30 11:50_

---

_Label `fixes` added by @MichaReiser on 2025-11-30 11:50_

---

_Comment by @typenoob on 2025-12-08 01:25_

Which python version and which anyio you were using?  I can't reproduce this.
```cmd
>>> import anyio
>>> anyio.lowlevel.checkpoint()
<coroutine object checkpoint at 0x00000250A45904C0>
```

---

_Comment by @injust on 2025-12-08 01:35_

Just a typing issue, my bad. Updated OP.

---
