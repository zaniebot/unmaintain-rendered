---
number: 7793
title: Add pdb / set_trace() check
type: issue
state: closed
author: giampaolo
labels:
  - question
assignees: []
created_at: 2023-10-03T19:28:51Z
updated_at: 2023-10-03T19:43:57Z
url: https://github.com/astral-sh/ruff/issues/7793
synced_at: 2026-01-07T13:12:15-06:00
---

# Add pdb / set_trace() check

---

_Issue opened by @giampaolo on 2023-10-03 19:28_

Basically this: https://pypi.org/project/flake8-debug/. Right now ruff is able to identify the `breakpoint` builtin function, which results in:

```
setup.py:29:13: T100 Trace found: `breakpoint` used
```

`breakpoint` builtin is a relatively new addition though (python 3.7, see [doc](https://docs.python.org/3/library/functions.html#breakpoint)). A lot of people (myself included) still use the `pdb` module, and may accidentally commit code like this:

```python
import pdb; pdb.set_trace()
```

This is particularly dangerous if the above code gets accidentally deployed in production.

---

_Label `rule` added by @zanieb on 2023-10-03 19:30_

---

_Comment by @zanieb on 2023-10-03 19:30_

Makes sense to me!

---

_Label `help wanted` added by @zanieb on 2023-10-03 19:31_

---

_Comment by @tjkuson on 2023-10-03 19:39_

I believe the rule `debugger` already catches this case.

```python
import pdb; pdb.set_trace()
```

triggers the rule on my machine.

---

_Comment by @zanieb on 2023-10-03 19:41_

Can confirm that `T100` is raised here 

>example.py:1:13: T100 Trace found: `pdb.set_trace` used

---

_Label `rule` removed by @zanieb on 2023-10-03 19:41_

---

_Label `help wanted` removed by @zanieb on 2023-10-03 19:41_

---

_Label `question` added by @zanieb on 2023-10-03 19:41_

---

_Comment by @giampaolo on 2023-10-03 19:43_

Whops! I must have done something wrong in my testing since I also confirm `pdb.set_trace` results in `T100` being raised. Sorry about the noise. :) Closing this out.

---

_Closed by @giampaolo on 2023-10-03 19:43_

---
