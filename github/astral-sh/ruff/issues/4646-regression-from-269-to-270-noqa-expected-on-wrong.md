---
number: 4646
title: "regression from 269 to 270: noqa expected on wrong line for B024"
type: issue
state: closed
author: atom-andrew
labels: []
assignees: []
created_at: 2023-05-24T22:01:39Z
updated_at: 2023-05-24T22:16:15Z
url: https://github.com/astral-sh/ruff/issues/4646
synced_at: 2026-01-07T13:12:14-06:00
---

# regression from 269 to 270: noqa expected on wrong line for B024

---

_Issue opened by @atom-andrew on 2023-05-24 22:01_

```
from abc import ABC
from dataclasses import dataclass


@dataclass
class Foo(ABC):  # noqa: B024
    """ Foo """
```

ruff check passes on v0.0.269 and fails on v0.0.270 with:

```
file.py:5:1: B024 `Foo` is an abstract base class, but it has no abstract methods
```

If the noqa is moved up to the line with @dataclass, it passes.

---

_Comment by @charliermarsh on 2023-05-24 22:06_

Thank you, sorry about that. It'll be fixed in the next release.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-24 22:08_

---

_Referenced in [astral-sh/ruff#4647](../../astral-sh/ruff/pulls/4647.md) on 2023-05-24 22:09_

---

_Closed by @charliermarsh on 2023-05-24 22:16_

---
