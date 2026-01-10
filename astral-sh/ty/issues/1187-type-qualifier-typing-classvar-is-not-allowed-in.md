```yaml
number: 1187
title: "Type qualifier `typing.ClassVar` is not allowed in type expressions (only in annotation expressions)"
type: issue
state: closed
author: dmytro-GL
labels:
  - bug
assignees: []
created_at: 2025-09-15T09:49:35Z
updated_at: 2025-09-15T11:16:58Z
url: https://github.com/astral-sh/ty/issues/1187
synced_at: 2026-01-10T02:06:25Z
```

# Type qualifier `typing.ClassVar` is not allowed in type expressions (only in annotation expressions)

---

_Issue opened by @dmytro-GL on 2025-09-15 09:49_

### Summary

`ClassVar` works fine, but `typing.ClassVar` doesn't, looks like a false positive.

```
import typing

from typing import ClassVar

import peewee


class A(peewee.Model):
    Name = peewee.CharField(255)

    class Meta:
        constraints: typing.ClassVar = [
            peewee.SQL("UNIQUE (Name)"),
        ]


class B(peewee.Model):
    Name = peewee.CharField(255)

    class Meta:
        constraints: ClassVar = [
            peewee.SQL("UNIQUE (Name)"),
        ]
```

```
$ uvx ty@0.0.1a20 check x.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                 error[invalid-type-form]: Type qualifier `typing.ClassVar` is not allowed in type expressions (only in annotation expressions)
  --> x.py:12:22
   |
11 |     class Meta:
12 |         constraints: typing.ClassVar = [
   |                      ^^^^^^^^^^^^^^^
13 |             peewee.SQL("UNIQUE (Name)"),
14 |         ]
   |
info: rule `invalid-type-form` is enabled by default

Found 1 diagnostic
```

RUF012 (https://docs.astral.sh/ruff/rules/mutable-class-default/) seems to be happy in both cases.


### Version

0.0.1a20

---

_Label `bug` added by @AlexWaygood on 2025-09-15 10:56_

---

_Closed by @AlexWaygood on 2025-09-15 11:06_

---

_Comment by @sharkdp on 2025-09-15 11:16_

Thank you for reporting this!

---
