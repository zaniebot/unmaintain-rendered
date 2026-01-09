---
number: 20373
title: Documentation DTZ007 conflicts with UP017
type: issue
state: closed
author: Jmandarino
labels:
  - question
assignees: []
created_at: 2025-09-12T21:35:02Z
updated_at: 2025-09-16T17:48:12Z
url: https://github.com/astral-sh/ruff/issues/20373
synced_at: 2026-01-07T13:12:16-06:00
---

# Documentation DTZ007 conflicts with UP017

---

_Issue opened by @Jmandarino on 2025-09-12 21:35_

### Summary

Hello, Not sure if this is the right place but 
[DTZ007](https://docs.astral.sh/ruff/rules/call-datetime-strptime-without-zone/) reccomends
```python
import datetime

datetime.datetime.strptime("2022/01/31", "%Y/%m/%d").replace(
    tzinfo=datetime.timezone.utc
)
``` 

which seems outdated by UP017


### Version

_No response_

---

_Comment by @ntBre on 2025-09-12 21:44_

There is a note about this at the very end of the DTZ007 `Example` section:

> On Python 3.11 and later, datetime.timezone.utc can be replaced with datetime.UTC.

We could consider updating the example code block and switching this note with something like "Before Python 3.11...," but I think it kind of makes sense to use an example that works on earlier Python versions too. Our default Python version is still 3.9, after all.

---

_Label `question` added by @ntBre on 2025-09-12 21:44_

---

_Comment by @Jmandarino on 2025-09-16 17:48_

yeah that makes sense to me! thanks for following up

---

_Closed by @Jmandarino on 2025-09-16 17:48_

---
