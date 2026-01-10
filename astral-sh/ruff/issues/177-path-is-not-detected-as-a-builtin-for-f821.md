```yaml
number: 177
title: __path__ is not detected as a builtin for F821
type: issue
state: closed
author: sanga
labels:
  - bug
assignees: []
created_at: 2022-09-13T06:53:29Z
updated_at: 2023-02-03T18:28:28Z
url: https://github.com/astral-sh/ruff/issues/177
synced_at: 2026-01-10T11:09:42Z
```

# __path__ is not detected as a builtin for F821

---

_Issue opened by @sanga on 2022-09-13 06:53_

Eg

`print(__path__)`

Produces "F821 Undefined name `__path__`"

---

_Label `bug` added by @charliermarsh on 2022-09-13 13:41_

---

_Comment by @charliermarsh on 2022-09-13 13:41_

Will fix :)

---

_Comment by @charliermarsh on 2022-09-13 14:13_

Can you give a bit more context on where `__path__` _should_ be considered defined? I think it's only present when in an `__init__.py` module, or something like that...

---

_Comment by @sanga on 2022-09-14 13:54_

I'm actually not sure without checking the docs to be honest. Definitely at least in `__init__.py` - that's where I hit this bug. But it's ok in any module that's part of a package too isn't it? This is the best link I could find from pythons docs https://docs.python.org/3/reference/import.html#module-path

---

_Added to milestone `Release 0.1.0` by @charliermarsh on 2022-09-15 13:28_

---

_Closed by @charliermarsh on 2022-09-15 13:44_

---

_Comment by @charliermarsh on 2022-09-15 13:44_

Thanks, you were totally right!

---
