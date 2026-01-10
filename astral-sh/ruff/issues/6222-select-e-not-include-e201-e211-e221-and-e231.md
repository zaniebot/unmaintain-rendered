---
number: 6222
title: select E not include E201, E211, E221 and E231?
type: issue
state: closed
author: OmmyZhang
labels: []
assignees: []
created_at: 2023-08-01T04:13:15Z
updated_at: 2023-08-01T04:28:19Z
url: https://github.com/astral-sh/ruff/issues/6222
synced_at: 2026-01-10T01:22:45Z
---

# select E not include E201, E211, E221 and E231?

---

_Issue opened by @OmmyZhang on 2023-08-01 04:13_

```console
$ ruff check c.py --select E
```
 shows no error, while

```console
$ ruff check c.py --select E231
c.py:1:7: E231 [*] Missing whitespace after ','
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

The same result if I use pyproject.toml to set select.

version: ruff 0.0.280 or main

c.py:
```python
a = (1,2)
```

I'm not sure if it's a bug or I don't use ruff in right way.

---

_Comment by @OmmyZhang on 2023-08-01 04:21_

I see, E231 seems still in Nursery.

---

_Renamed from "select E not include Exxx?" to "select E not include E201, E211, E221 and E231?" by @OmmyZhang on 2023-08-01 04:23_

---

_Closed by @OmmyZhang on 2023-08-01 04:28_

---
