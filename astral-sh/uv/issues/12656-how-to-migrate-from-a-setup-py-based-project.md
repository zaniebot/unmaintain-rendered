---
number: 12656
title: How to migrate from a setup.py based project?
type: issue
state: closed
author: phaabe
labels:
  - question
assignees: []
created_at: 2025-04-03T15:50:18Z
updated_at: 2025-04-04T12:57:36Z
url: https://github.com/astral-sh/uv/issues/12656
synced_at: 2026-01-10T01:25:22Z
---

# How to migrate from a setup.py based project?

---

_Issue opened by @phaabe on 2025-04-03 15:50_

### Question

I have searched the docs and googled around.

This is the best solution I can think of so far: Simply migrate the dependencies and do the rest by hand:

```
uv pip compile setup.py -o requirements.txt
uv add -r requirements.txt
```

But I am not sure if I missed actual tooling for this or if there are better ways. In my case the `setup.py` is quite simple, but other cases may vary. 


---

_Label `question` added by @phaabe on 2025-04-03 15:50_

---

_Comment by @zanieb on 2025-04-03 16:26_

That seems reasonable, I'm not sure what else I could recommend right now.

---

_Closed by @charliermarsh on 2025-04-04 12:35_

---

_Comment by @phaabe on 2025-04-04 12:57_

Thanks for the reply!

---
