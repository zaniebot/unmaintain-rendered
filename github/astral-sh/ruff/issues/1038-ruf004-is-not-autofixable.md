---
number: 1038
title: RUF004 is not autofixable
type: issue
state: closed
author: LefterisJP
labels: []
assignees: []
created_at: 2022-12-04T16:19:38Z
updated_at: 2022-12-04T17:04:27Z
url: https://github.com/astral-sh/ruff/issues/1038
synced_at: 2026-01-07T13:12:14-06:00
---

# RUF004 is not autofixable

---

_Issue opened by @LefterisJP on 2022-12-04 16:19_

This is with ruff `0.0.155`

Consider the following:

```python
a = 1
while a < 10:
    a += 1

exit()
```

running ruff on it with RUF004 activated works and detects the problem.
But if you try to fix it with `ruff --fix` it does not autofix it. And afaics this should be autofixable.

---

_Comment by @charliermarsh on 2022-12-04 16:27_

It’s because sys isn’t imported, and we don’t have the ability to add it to the import list (yet) since edits are currently localized.

---

_Comment by @charliermarsh on 2022-12-04 16:28_

(Or, that’s my guess — I’m on the subway :))

---

_Comment by @LefterisJP on 2022-12-04 16:39_

No worries :)

Do you think it's something ruff can fix at some point?

---

_Comment by @charliermarsh on 2022-12-04 17:01_

Yes absolutely!

---

_Comment by @charliermarsh on 2022-12-04 17:02_

It’s tracked somewhat obliquely by #835 so I’m going to close this one out in favor of the existing issue.

---

_Closed by @charliermarsh on 2022-12-04 17:02_

---

_Comment by @LefterisJP on 2022-12-04 17:04_

> It’s tracked somewhat obliquely by #835 so I’m going to close this one out in favor of the existing issue.

Sure no problem. I fixed it in our case by adding import sys manually and changing one exit to sys.exit(). Then ruff did autofix all the rest on its own ;)

---
