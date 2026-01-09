---
number: 1787
title: "PIE794 fixer removes wrong code and doens't detect one use case"
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-11T20:32:22Z
updated_at: 2023-01-11T23:24:31Z
url: https://github.com/astral-sh/ruff/issues/1787
synced_at: 2026-01-07T13:12:14-06:00
---

# PIE794 fixer removes wrong code and doens't detect one use case

---

_Issue opened by @spaceone on 2023-01-11 20:32_

ruff 0.0.218
`ruff --select PIE794 foo.py`
```
class foo:
    bar = 1 
    bar = 2
```
→ not detected at all


`ruff --select PIE794 --fix foo.py`
```
class foo(object):
    bar = 1 
    bar = 2
```
→ changes into:
```
class foo(object):
    bar = 1 
    
```
1. this is wrong. bar should be `2`
2. it leaves 4 trailing spaces behind (`W293`)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-11 20:35_

---

_Label `autofix` added by @charliermarsh on 2023-01-11 20:36_

---

_Comment by @charliermarsh on 2023-01-11 20:36_

Thank you! All correct. Will fix.

---

_Comment by @charliermarsh on 2023-01-11 20:43_

(Will hopefully get to this later today.)

---

_Comment by @charliermarsh on 2023-01-11 23:00_

I think it's not clear that `bar = 1` should be removed instead of `bar = 2`. I know that semantically, it's correct, in that `bar = 2` is the value that's resolved when you run the code. But I feel like this check is more likely to occur in situations in which you redefine a field that you already defined? In which case, the second instance would be the "error"?

---

_Referenced in [astral-sh/ruff#1794](../../astral-sh/ruff/pulls/1794.md) on 2023-01-11 23:01_

---

_Closed by @charliermarsh on 2023-01-11 23:01_

---

_Comment by @charliermarsh on 2023-01-11 23:01_

I fixed the other two issues in #1794. We can keep discussing since the last thing is sort of subjective.

---

_Comment by @spaceone on 2023-01-11 23:17_

> I think it's not clear that `bar = 1` should be removed instead of `bar = 2`. I know that semantically, it's correct, in that `bar = 2` is the value that's resolved when you run the code. But I feel like this check is more likely to occur in situations in which you redefine a field that you already defined? In which case, the second instance would be the "error"?

hm, I don't want to risk a behavior change by using the autofixers, so I would change that to the semantically-correct version. If it was wrong before the linter shows you this and you can act upon it.

---

_Comment by @charliermarsh on 2023-01-11 23:24_

Yeah that's a reasonable argument. Okay, I'll do another pass on this.

---

_Referenced in [astral-sh/ruff#2390](../../astral-sh/ruff/issues/2390.md) on 2023-01-31 12:26_

---
