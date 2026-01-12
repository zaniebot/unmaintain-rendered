```yaml
number: 1926
title: "Pyupgrade: Extraneous parenthesis"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: ExtraneousParenthesis
created_at: 2023-01-17T01:10:53Z
updated_at: 2023-01-20T05:05:03Z
url: https://github.com/astral-sh/ruff/pull/1926
synced_at: 2026-01-12T15:55:07Z
```

# Pyupgrade: Extraneous parenthesis

---

_@colin99d_

A part of #827. Leaving this as a draft for visibility.

---

_Marked ready for review by @colin99d on 2023-01-19 22:24_

---

_Comment by @colin99d on 2023-01-19 22:27_

This should be ready for review. I made one difference with the original library. The command is called extraneous prints, so I only ran it on print functions; however based on the test below, I don't think the one in pyupgrade runs on prints only. Would you like me to convert it to run on any function?

```
def f():
    x = int(((yield 1)))
```

---

_Comment by @charliermarsh on 2023-01-19 22:29_

Yeah, probably ok to run on any function.

---

_Comment by @colin99d on 2023-01-19 22:42_

Changes in!

---

_Comment by @colin99d on 2023-01-19 22:44_

Oh shoot, I missed one big bug. Let me fix real quick!

---

_Converted to draft by @colin99d on 2023-01-19 22:44_

---

_Comment by @colin99d on 2023-01-20 00:55_

Also, I will note that I spent a decent amount of time trying to use libcst_native instead of format! for this, but the solution was going to be really complicated.

---

_Comment by @charliermarsh on 2023-01-20 01:23_

Sounds good. Mark as ready whenever it's good for a review.

---

_Comment by @colin99d on 2023-01-20 02:46_

Its ready!

---

_Marked ready for review by @colin99d on 2023-01-20 02:46_

---

_Merged by @charliermarsh on 2023-01-20 05:04_

---

_Closed by @charliermarsh on 2023-01-20 05:04_

---

_@charliermarsh reviewed on 2023-01-20 05:05_

---

_Review comment by @charliermarsh on `src/rules/pyupgrade/rules/extraneous_parentheses.rs`:11 on 2023-01-20 05:05_

@colin99d - I decided to instead do a straight port of the pyupgrade implementation based on the token stream. I didn't want to ask you to make big changes like this since you've already put in some work on these PRs. I hope that's cool though -- sorry if I should've asked first.

---
