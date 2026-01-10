```yaml
number: 54
title: Crash on parenthesized with statement
type: issue
state: closed
author: antonagestam
labels:
  - bug
assignees: []
created_at: 2022-08-31T08:38:05Z
updated_at: 2023-02-03T18:28:27Z
url: https://github.com/astral-sh/ruff/issues/54
synced_at: 2026-01-10T11:09:42Z
```

# Crash on parenthesized with statement

---

_Issue opened by @antonagestam on 2022-08-31 08:38_

I tried running ruff on a large project, which resulted in this crash:

```
[2022-08-31][10:33:40][ruff][ERROR] Failed to check repr-with.py: invalid syntax. Got unexpected token 'as' at line 9 column 13
```

It seems like the crash happens on parenthesized context managers introduced in Python 3.10. Minimal reproducible example:

```python
from contextlib import contextmanager

@contextmanager
def ctx():
    yield

with (ctx() as foo):
    ...
```

---

_Comment by @charliermarsh on 2022-08-31 12:19_

Thanks for reporting! Looks like a bug in the parser. I'll look into it :)

---

_Comment by @charliermarsh on 2022-08-31 13:21_

Will come back to this, but reported in the parser here too: https://github.com/RustPython/RustPython/issues/4145

---

_Label `bug` added by @charliermarsh on 2022-08-31 13:21_

---

_Added to milestone `Release 0.1.0` by @charliermarsh on 2022-09-15 13:44_

---

_Comment by @charliermarsh on 2022-09-21 16:26_

(This is a blocker for 0.1.0.)

---

_Closed by @charliermarsh on 2022-12-13 15:16_

---
