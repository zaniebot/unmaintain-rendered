---
number: 11281
title: "`ARG001` False positive warning about unsed `args`, `kwargs`"
type: issue
state: closed
author: mh-firouzjah
labels:
  - question
assignees: []
created_at: 2024-05-04T14:48:14Z
updated_at: 2024-05-22T04:15:59Z
url: https://github.com/astral-sh/ruff/issues/11281
synced_at: 2026-01-07T13:12:15-06:00
---

# `ARG001` False positive warning about unsed `args`, `kwargs`

---

_Issue opened by @mh-firouzjah on 2024-05-04 14:48_

While *args and **kwargs are commonly used to handle variable-length arguments and keyword arguments, there are cases where we intentionally do not use these variables directly inside the function body. For example, when overriding existing methods or implementing callback functions, *args and **kwargs may be part of the function signature for compatibility or flexibility without immediate usage in the implementation. In such scenarios, the apparent unused status of *args and **kwargs is intentional and not indicative of an issue. This linting warning can be considered a false positive in specific design contexts.

---

_Comment by @charliermarsh on 2024-05-04 17:34_

What's the specific false positive that you're seeing? Note that if you mark a method with [`@typing.override`](https://docs.python.org/3/library/typing.html#typing.override), we already ignore `ARG` violations (same with methods marked as abstract, etc.).

---

_Label `question` added by @charliermarsh on 2024-05-04 17:34_

---

_Comment by @charliermarsh on 2024-05-22 04:15_

Oh, we have a setting for this, I think? [`ignore-variadic-names`](https://docs.astral.sh/ruff/settings/#lint_flake8-unused-arguments_ignore-variadic-names)

---

_Closed by @charliermarsh on 2024-05-22 04:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-22 04:15_

---
