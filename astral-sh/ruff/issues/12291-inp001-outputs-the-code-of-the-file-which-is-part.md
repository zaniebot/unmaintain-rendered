```yaml
number: 12291
title: INP001 outputs the code of the file which is part of an implicit namespace package
type: issue
state: closed
author: adamtheturtle
labels:
  - cli
assignees: []
created_at: 2024-07-11T18:02:52Z
updated_at: 2024-07-12T15:21:30Z
url: https://github.com/astral-sh/ruff/issues/12291
synced_at: 2026-01-12T15:54:51Z
```

# INP001 outputs the code of the file which is part of an implicit namespace package

---

_@adamtheturtle_

~ruff 0.4.10~ Edit: I pasted wrong! `ruff 0.5.1`

This makes me think that the code has an error in it, that I am looking for, but that is not the case.

---

_Comment by @zanieb on 2024-07-11 18:05_

Hi! Can you share a brief example?

---

_Comment by @adamtheturtle on 2024-07-11 18:12_

Sure @zanieb ,

I create:

`foo/bar/example.py`

with the contents:

```python
STRING = "This code is not relevant"
STRING1 = "This code is not relevant"
STRING2 = "This code is not relevant"
STRING3 = "This code is not relevant"
```

```
> ruff check foo/bar/
foo/bar/example.py:1:1: INP001 File `foo/bar/example.py` is part of an implicit namespace package. Add an `__init__.py`.
  |
1 | STRING = "This code is not relevant"
  |  INP001
2 | STRING1 = "This code is not relevant"
3 | STRING2 = "This code is not relevant"
  |
```

I infer from the output that there is a problem to be fixed on line 1 of the file.

---

_Comment by @dhruvmanila on 2024-07-12 03:55_

I think file level diagnostics by default uses an empty range at the start of the file which is what you're seeing here.

---

_Label `cli` added by @dhruvmanila on 2024-07-12 03:56_

---

_Comment by @charliermarsh on 2024-07-12 13:01_

Lets omit the code frame for empty files?

---

_Comment by @dhruvmanila on 2024-07-12 13:08_

> Lets omit the code frame for empty files?

Yeah, that seems reasonable. I think we would want to check for an empty range at byte 0 (`TextRange::new(0, 0)`) which is where this is being raised.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-12 13:18_

---

_Closed by @charliermarsh on 2024-07-12 15:21_

---
