```yaml
number: 13754
title: "SIM300 (yoda-conditions) false positive: `assert X < datetime.timedelta(...)`"
type: issue
state: closed
author: rdaysky
labels:
  - question
assignees: []
created_at: 2024-10-15T02:36:37Z
updated_at: 2024-11-26T03:36:36Z
url: https://github.com/astral-sh/ruff/issues/13754
synced_at: 2026-01-12T15:54:53Z
```

# SIM300 (yoda-conditions) false positive: `assert X < datetime.timedelta(...)`

---

_@rdaysky_

I implemented an algorithm that can only handle certain values of a parameter. To make sure future updates to the code don’t bring it outside the allowed range, I added an assertion. This triggered SIM300:
```
DURATION = datetime.timedelta(hours=1)

assert DURATION < datetime.timedelta(days=1), "algorithm breaks on large durations"
```
Ruff apparently thinks that a call to a class constructor is the thing being tested and the constant is the reference value, while actually it’s the other way round. Not sure how the definition of the rule could be updated so it would see things like this.

---

_Label `bug` added by @MichaReiser on 2024-10-22 06:40_

---

_Label `help wanted` added by @MichaReiser on 2024-10-22 06:40_

---

_Comment by @charliermarsh on 2024-11-09 19:39_

I think the rule is "correct" in that the all-capitalized `DURATION` is typically used as the convention to indicate a constant. Why stylize it that way?

---

_Label `bug` removed by @charliermarsh on 2024-11-09 19:39_

---

_Label `help wanted` removed by @charliermarsh on 2024-11-09 19:39_

---

_Label `question` added by @charliermarsh on 2024-11-09 19:39_

---

_Comment by @rdaysky on 2024-11-09 19:47_

It follows the convention to indicate a constant because it is a constant. And the subsequent assertion compares it against another constant.

---

_Comment by @charliermarsh on 2024-11-09 19:59_

I think I understand what you're doing but it's a fairly unusual test. You're validating that a statically-defined constant is less than some threshold value? I don't know that it makes sense to adjust the rule in favor of this, it seems pretty rare.

---

_Closed by @charliermarsh on 2024-11-26 03:36_

---
