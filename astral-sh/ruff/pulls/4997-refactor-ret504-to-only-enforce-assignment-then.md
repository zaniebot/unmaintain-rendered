```yaml
number: 4997
title: "Refactor `RET504` to only enforce assignment-then-return pattern"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/return
created_at: 2023-06-10T02:42:45Z
updated_at: 2023-06-10T04:05:03Z
url: https://github.com/astral-sh/ruff/pull/4997
synced_at: 2026-01-12T03:43:29Z
```

# Refactor `RET504` to only enforce assignment-then-return pattern

---

_Pull request opened by @charliermarsh on 2023-06-10 02:42_

## Summary

The `RET504` rule, which looks for unnecessary assignments before return statements, is a frequent source of issues (#4173, #4236, #4242, #1606, #2950). Over time, we've tried to refine the logic to handle more cases. For example, we now avoid analyzing any functions that contain any function calls or attribute assignments, since those operations can contain side effects (and so we mark them as a "read" on all variables in the function -- we could do a better job with code graph analysis to handle this limitation, but that'd be a more involved change.) We also avoid flagging any variables that are the target of multiple assignments. Ultimately, though, I'm not happy with the implementation -- we just can't do sufficiently reliable analysis of arbitrary code flow given the limited logic herein, and the existing logic is very hard to reason about and maintain.

This PR refocuses the rule to only catch cases of the form:

```py
def f():
    x = 1
    return x
```

That is, we now only flag returns that are immediately preceded by an assignment to the returned variable. While this is more limiting, in some ways, it lets us flag more cases vis-a-vis the previous implementation, since we no longer "fully eject" when functions contain function calls and other effect-ful operations.

Closes #4173.

Closes #4236.

Closes #4242.


---

_Label `rule` added by @charliermarsh on 2023-06-10 02:42_

---

_Merged by @charliermarsh on 2023-06-10 04:05_

---

_Closed by @charliermarsh on 2023-06-10 04:05_

---

_Branch deleted on 2023-06-10 04:05_

---
