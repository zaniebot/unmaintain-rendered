---
number: 8667
title: "`self-assigning-variable` (`PLW0127`) false negative on multi assignment"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-11-14T00:12:31Z
updated_at: 2023-11-25T18:42:21Z
url: https://github.com/astral-sh/ruff/issues/8667
synced_at: 2026-01-07T13:12:15-06:00
---

# `self-assigning-variable` (`PLW0127`) false negative on multi assignment

---

_Issue opened by @DetachHead on 2023-11-14 00:12_

```py
a = 1
a = a # error

b = b = 1 # no error
```
[playground](https://play.ruff.rs/71ab81ea-2846-4e7e-9c22-b1978ed84ace)

---

_Comment by @charliermarsh on 2023-11-14 00:14_

Should the issue title be "false negative"? I want to make sure I understand the issue.

---

_Label `bug` added by @charliermarsh on 2023-11-14 00:14_

---

_Renamed from "`self-assigning-variable` (`PLW0127`) false positive on multi assignment" to "`self-assigning-variable` (`PLW0127`) negative positive on multi assignment" by @DetachHead on 2023-11-14 00:15_

---

_Comment by @DetachHead on 2023-11-14 00:15_

yeah my bad

---

_Renamed from "`self-assigning-variable` (`PLW0127`) negative positive on multi assignment" to "`self-assigning-variable` (`PLW0127`) false negative on multi assignment" by @DetachHead on 2023-11-14 00:16_

---

_Comment by @charliermarsh on 2023-11-14 00:17_

No worries, thanks!

---

_Label `good first issue` added by @charliermarsh on 2023-11-14 00:17_

---

_Comment by @DetachHead on 2023-11-14 01:23_

if this is a https://github.com/astral-sh/ruff/labels/good%20first%20issue i'll have a go at fixing it myself

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-25 18:21_

---

_Referenced in [astral-sh/ruff#8839](../../astral-sh/ruff/pulls/8839.md) on 2023-11-25 18:27_

---

_Closed by @charliermarsh on 2023-11-25 18:42_

---
