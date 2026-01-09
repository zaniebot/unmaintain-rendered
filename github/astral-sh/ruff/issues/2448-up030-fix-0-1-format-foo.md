---
number: 2448
title: "UP030: fix `'_{0}._{1}'.format(*foo)`"
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-02-01T17:17:24Z
updated_at: 2023-02-04T22:34:49Z
url: https://github.com/astral-sh/ruff/issues/2448
synced_at: 2026-01-07T13:12:14-06:00
---

# UP030: fix `'_{0}._{1}'.format(*foo)`

---

_Issue opened by @spaceone on 2023-02-01 17:17_

`'_{0}._{1}'.format(*foo)` can't be autofixed.

If the order 0 and 1 is correct it should simply be changeable into:
`'_{}._{}'.format(*foo)` 

---

_Comment by @colin99d on 2023-02-03 01:24_

Thanks for catching this! I will work on it after I finish my current PR.

---

_Label `rule` added by @charliermarsh on 2023-02-03 18:16_

---

_Label `autofix` added by @charliermarsh on 2023-02-03 18:16_

---

_Label `rule` removed by @charliermarsh on 2023-02-03 18:16_

---

_Referenced in [astral-sh/ruff#2568](../../astral-sh/ruff/pulls/2568.md) on 2023-02-04 15:15_

---

_Closed by @charliermarsh on 2023-02-04 22:34_

---
