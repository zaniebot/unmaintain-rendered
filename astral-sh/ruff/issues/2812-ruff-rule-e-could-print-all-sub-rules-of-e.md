```yaml
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
synced_at: 2026-01-12T15:54:43Z
```

# `ruff rule E` could print all sub-rules of E

---

_@spaceone_

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

_Closed by @charliermarsh on 2023-09-21 02:05_

---
