---
number: 4534
title: Avoid ambiguous-unicode errors for all-unicode strings
type: issue
state: closed
author: charliermarsh
labels:
  - rule
  - help wanted
assignees: []
created_at: 2023-05-19T18:20:17Z
updated_at: 2023-05-22T14:42:27Z
url: https://github.com/astral-sh/ruff/issues/4534
synced_at: 2026-01-07T13:12:14-06:00
---

# Avoid ambiguous-unicode errors for all-unicode strings

---

_Issue opened by @charliermarsh on 2023-05-19 18:20_

Well, the title is a little misleading -- there's more nuance to it.

If you look at VS Code (https://github.com/microsoft/vscode/issues/143720#issuecomment-1048757234, https://github.com/microsoft/vscode/issues/143796#issuecomment-1049773616), their logic for flagging ambiguous-unicode characters takes into account the context.

In short, we continue to flag ambiguous-unicode _except_ if (1) the "word" is all non-ascii, and (2) the "word" contains at least one non-confusable unicode character.


---

_Label `rule` added by @charliermarsh on 2023-05-19 18:20_

---

_Label `help wanted` added by @charliermarsh on 2023-05-19 18:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-20 18:58_

---

_Referenced in [astral-sh/ruff#4552](../../astral-sh/ruff/pulls/4552.md) on 2023-05-20 20:51_

---

_Closed by @charliermarsh on 2023-05-22 14:42_

---
