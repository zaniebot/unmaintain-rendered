---
number: 4050
title: False B031 error when using generator from itertools.groupby in if/else block
type: issue
state: closed
author: premdas26
labels:
  - bug
assignees: []
created_at: 2023-04-20T17:58:39Z
updated_at: 2023-04-23T06:04:17Z
url: https://github.com/astral-sh/ruff/issues/4050
synced_at: 2026-01-07T13:12:14-06:00
---

# False B031 error when using generator from itertools.groupby in if/else block

---

_Issue opened by @premdas26 on 2023-04-20 17:58_

After upgrading to v0.0.262, I'm getting a false B031 error for multiple uses of a generator from itertools.groupby:
```
x = dict()    
for key, group in itertools.groupby(x, lambda x: x):
  if key:
    for n in group:
      print(n)
  else:
      list(group)
```
This code will result in `B031 Using the generator returned from itertools.groupby() more than once will do nothing on the second usage`. However, if I do not have a loop in the if block, I do not get the error, so as far as I can tell, it seems to be specific to loops (including comprehensions). This appears to be a case not caught by the fix in [#3801](https://github.com/charliermarsh/ruff/issues/3801)


---

_Comment by @dhruvmanila on 2023-04-20 18:19_

Interesting! I see the problem. The global count is being increased instead of the branch local count. This leads to updating the global count twice which means that the line `list(group)` will be where this false positive is being detected at.

Let me take a look at it, I think even comprehensions might be affected.

---

_Label `bug` added by @charliermarsh on 2023-04-21 01:30_

---

_Comment by @premdas26 on 2023-04-21 17:04_

great thank you!

---

_Assigned to @dhruvmanila by @MichaReiser on 2023-04-23 04:14_

---

_Referenced in [astral-sh/ruff#4070](../../astral-sh/ruff/pulls/4070.md) on 2023-04-23 05:51_

---

_Closed by @charliermarsh on 2023-04-23 06:04_

---

_Referenced in [astral-sh/ruff#10337](../../astral-sh/ruff/issues/10337.md) on 2024-03-11 12:06_

---
