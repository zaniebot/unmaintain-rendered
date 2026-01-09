---
number: 5665
title: "Add support for `B034` from `flake8-bugbear`"
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
assignees: []
created_at: 2023-07-10T18:48:50Z
updated_at: 2023-07-11T03:52:57Z
url: https://github.com/astral-sh/ruff/issues/5665
synced_at: 2026-01-07T13:12:15-06:00
---

# Add support for `B034` from `flake8-bugbear`

---

_Issue opened by @dhruvmanila on 2023-07-10 18:48_

Ref: https://github.com/PyCQA/flake8-bugbear/releases/tag/23.7.10

> B034: Calls to re.sub, re.subn or re.split should pass flags or count/maxsplit as keyword arguments. It is commonly assumed that flags is the third positional parameter, forgetting about count/maxsplit, since many other re module functions are of the form f(pattern, string, flags).

---

_Label `rule` added by @dhruvmanila on 2023-07-10 18:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-11 02:20_

---

_Referenced in [astral-sh/ruff#5669](../../astral-sh/ruff/pulls/5669.md) on 2023-07-11 02:44_

---

_Closed by @charliermarsh on 2023-07-11 03:52_

---
