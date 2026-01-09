---
number: 2812
title: "`ruff rule E` could print all sub-rules of E"
type: issue
state: closed
author: spaceone
labels:
  - cli
assignees: []
created_at: 2023-02-12T12:34:05Z
updated_at: 2023-09-21T02:05:41Z
url: https://github.com/astral-sh/ruff/issues/2812
synced_at: 2026-01-07T13:12:14-06:00
---

# `ruff rule E` could print all sub-rules of E

---

_Issue opened by @spaceone on 2023-02-12 12:34_

```sh
$ ruff rule E1
error: invalid value 'E1' for '<RULE>': unknown rule code
```

→ nice would be if all sub-rules would be printed.

---

_Label `cli` added by @charliermarsh on 2023-02-12 15:43_

---

_Comment by @ngnpope on 2023-02-12 15:55_

This could be rather a lot of output. Might be nice to implement pager support?

---

_Renamed from "ruff rule E could print all sub-rules" to "`ruff rule E` could print all sub-rules of E" by @spaceone on 2023-02-12 21:39_

---

_Referenced in [astral-sh/ruff#5495](../../astral-sh/ruff/issues/5495.md) on 2023-07-03 20:54_

---

_Referenced in [astral-sh/ruff#7560](../../astral-sh/ruff/pulls/7560.md) on 2023-09-21 01:29_

---

_Closed by @charliermarsh on 2023-09-21 02:05_

---
