```yaml
number: 5443
title: Black not compatible with ruff?
type: issue
state: closed
author: Ryang20718
labels:
  - question
assignees: []
created_at: 2023-06-29T16:14:44Z
updated_at: 2023-06-30T02:10:57Z
url: https://github.com/astral-sh/ruff/issues/5443
synced_at: 2026-01-10T11:09:47Z
```

# Black not compatible with ruff?

---

_Issue opened by @Ryang20718 on 2023-06-29 16:14_

Hi, appreciate all the great linter/autofixes, however, I was wondering if ruff is up to date with black?

![image](https://github.com/astral-sh/ruff/assets/31294356/547ddc96-4c75-449b-8507-8c58811830ee)

e.g with a file of 
```
k=""
```

it should be autoformatted to (getting this when running black, but not running ruff
```
| k = ""
 ```
 
 

---

_Comment by @zanieb on 2023-06-29 16:27_

Hey @Ryang20718 — Ruff does not perform autoformatting at this time. It is _compatible_ with Black such that running Black will not introduce violations in Ruff.

See https://github.com/astral-sh/ruff/issues/5385#issuecomment-1609789153 for a bit more detail.

---

_Label `question` added by @zanieb on 2023-06-29 16:28_

---

_Comment by @Ryang20718 on 2023-06-29 17:43_

I see. ok thanks for the clarification

---

_Closed by @charliermarsh on 2023-06-30 02:10_

---
