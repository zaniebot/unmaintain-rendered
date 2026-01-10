---
number: 1347
title: Cannot select specific flake8-errmsg checks
type: issue
state: closed
author: rhkleijn
labels: []
assignees: []
created_at: 2022-12-23T11:34:39Z
updated_at: 2022-12-23T13:25:21Z
url: https://github.com/astral-sh/ruff/issues/1347
synced_at: 2026-01-10T01:22:39Z
---

# Cannot select specific flake8-errmsg checks

---

_Issue opened by @rhkleijn on 2022-12-23 11:34_

In my project I want to enable EM102 and EM103 from flake8-errmsg, but not EM101.

However, enabling any of the flake8-errmsg checks seems to enable all of them.

For example, both
```bash
$ echo "raise Exception('a string literal')" | ruff - --select EM --ignore EM101
```
and
```bash
$ echo "raise Exception('a string literal')" | ruff - --select EM102,EM103
```
also enable the EM101 check:
```bash
-:1:17: EM101 Exception must not use a string literal, assign to variable first
```

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-23 13:16_

---

_Comment by @charliermarsh on 2022-12-23 13:17_

Ah yeah that's a legitimate oversight. Will fix this today (but not right now, gotta run).

---

_Referenced in [astral-sh/ruff#1349](../../astral-sh/ruff/pulls/1349.md) on 2022-12-23 13:24_

---

_Closed by @charliermarsh on 2022-12-23 13:25_

---

_Comment by @charliermarsh on 2022-12-23 13:25_

Ok I lied, I fixed it. It will go out in the next release.

---
