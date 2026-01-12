```yaml
number: 14378
title: DTZ901 Instead of warning replace datetime.datetime.min.time() with datetime.time.min
type: issue
state: closed
author: spaceby
labels:
  - bug
  - preview
assignees: []
created_at: 2024-11-16T12:31:26Z
updated_at: 2024-11-18T02:24:36Z
url: https://github.com/astral-sh/ruff/issues/14378
synced_at: 2026-01-12T15:54:53Z
```

# DTZ901 Instead of warning replace datetime.datetime.min.time() with datetime.time.min

---

_@spaceby_

datetime.datetime.min may cause a timezone bug, but datetime.datetime.min.time() does not cause timezone problems.

I propose to add an exception to DTZ901 for datetime.datetime.min.time() and datetime.datetime.max.time()
Or replace them with datetime.time.min and datetime.time.max

---

_Comment by @charliermarsh on 2024-11-16 17:56_

Can you expand on why `datetime.datetime.min.time()` is okay? (Not arguing that it isn't, just want to fully understand before we change it.)

---

_Label `rule` added by @MichaReiser on 2024-11-16 21:59_

---

_Label `preview` added by @MichaReiser on 2024-11-16 21:59_

---

_Comment by @InSyncWithFoo on 2024-11-17 00:57_

The docs for `.time()` is quite helpful here:

```pycon
>>> import datetime
>>> help(datetime.datetime.min.time)
Help on built-in function time:

time() method of datetime.datetime instance
    Return time object with same time but with tzinfo=None.
```

I'll work on a fix.

---

_Label `bug` added by @MichaReiser on 2024-11-17 09:35_

---

_Label `rule` removed by @MichaReiser on 2024-11-17 09:35_

---

_Comment by @spaceby on 2024-11-17 17:55_

> Can you expand on why `datetime.datetime.min.time()` is okay? (Not arguing that it isn't, just want to fully understand before we change it.)

datetime.datetime.min returns an object that can explode.
However, datetime.datetime.min.time() returns a normal time. There are no localization issues here. There is no problematic datetime here. There is no localization either.
Moreover, it is actively used in datetime.combine

It's strange that people use datetime.datetime.min.time() instead of datetime.time.min .
It's as if people look to the beginning of time to find out what time the day begins.

---

_Closed by @charliermarsh on 2024-11-18 02:24_

---
