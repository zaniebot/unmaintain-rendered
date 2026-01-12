```yaml
number: 9511
title: "Rule: Flake8-bugbear B038 \" detect changes to iterable object of loop\""
type: issue
state: closed
author: mimre25
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-01-14T05:40:35Z
updated_at: 2024-04-11T19:52:54Z
url: https://github.com/astral-sh/ruff/issues/9511
synced_at: 2026-01-12T15:54:49Z
```

# Rule: Flake8-bugbear B038 " detect changes to iterable object of loop"

---

_@mimre25_

I've added a new rule to flake8-bugbear.

In brief, the rule detects bugs like the following:
```py
some_list = [1,2,3]
for i in some_list:
  if i % 2 == 0:
    some_list.remove(i)
  print(i)
```

(More context at github.com/PyCQA/flake8-bugbear/issues/445)

I'd be happy to contribute if its desired to implement this rule in ruff as well :slightly_smiling_face: 

---

_Comment by @charliermarsh on 2024-01-14 15:11_

Definitely happy to include in Ruff, would welcome a contribution!

---

_Label `rule` added by @charliermarsh on 2024-01-14 15:11_

---

_Label `accepted` added by @charliermarsh on 2024-01-14 15:11_

---

_Renamed from "Rule: Flake8-bugbar B038 " detect changes to iterable object of loop"" to "Rule: Flake8-bugbear B038 " detect changes to iterable object of loop"" by @AlexWaygood on 2024-01-14 16:57_

---

_Comment by @mimre25 on 2024-01-14 17:08_

Sounds good - I hope I can get the PR during this week :slightly_smiling_face: 

---

_Assigned to @mimre25 by @charliermarsh on 2024-01-14 17:16_

---

_Comment by @charliermarsh on 2024-01-14 17:18_

Awesome :) I will assign you to avoid others hopping on it accidentally, but just LMK if you find yourself unable to get to it.

---

_Comment by @mimre25 on 2024-01-16 16:47_

FYI: I got a prototype working, but with flake8-bugbear we already got reports about some oversights leading to false positives - I'm looking into making it more solid - just wanted to give a heads up that I'm getting there :slightly_smiling_face: 

---

_Comment by @mimre25 on 2024-01-19 15:12_

PR is out: https://github.com/astral-sh/ruff/pull/9578

---

_Closed by @charliermarsh on 2024-04-11 19:52_

---
