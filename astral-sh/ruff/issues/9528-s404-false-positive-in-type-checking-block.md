```yaml
number: 9528
title: "`S404` false positive in `TYPE_CHECKING` block"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2024-01-15T10:47:03Z
updated_at: 2024-01-16T03:09:10Z
url: https://github.com/astral-sh/ruff/issues/9528
synced_at: 2026-01-12T15:54:49Z
```

# `S404` false positive in `TYPE_CHECKING` block

---

_@DetachHead_

```py
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from subprocess import run
```
https://play.ruff.rs/d722ef1d-09fd-46c7-82c5-23d85498c6ae

since it's only used when type-checking, there's no security risk so this rule should not be triggered

---

_Comment by @KotlinIsland on 2024-01-15 11:36_

Inb4 my voln repro does `typing.TYPE_CHECKING = True`

---

_Comment by @charliermarsh on 2024-01-15 14:46_

Torn on this because it's typically a false positive but it's also a security-oriented rule so it feels strange to be lax on it.

---

_Comment by @eli-schwartz on 2024-01-16 03:04_

I would assume that a typing-only import of the subprocess module indicates that the code in question either returns or accepts subprocess module types and is therefore "insecure" for this reason.

The fact that it was only detected via the type signature rather than via a direct import seems immaterial. The currently reported violation allows detecting things like:

```python
import typing as T

from fooutils import Popen_unicode
from myapp import load_settings

if T.TYPE_CHECKING:
    from subprocess import DEVNULL, PIPE

cfg = load_settings()
r, out, err = Popen_unicode(cfg['command'], stdout=PIPE, stderr=DEVNULL)
print(out)
```

---

_Closed by @DetachHead on 2024-01-16 03:09_

---
