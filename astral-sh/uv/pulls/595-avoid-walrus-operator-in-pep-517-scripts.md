```yaml
number: 595
title: Avoid walrus operator in PEP 517 scripts
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/walrus
created_at: 2023-12-09T01:21:46Z
updated_at: 2023-12-11T08:53:49Z
url: https://github.com/astral-sh/uv/pull/595
synced_at: 2026-01-10T15:44:44Z
```

# Avoid walrus operator in PEP 517 scripts

---

_Pull request opened by @charliermarsh on 2023-12-09 01:21_

I believe this unnecessarily puts a Python 3.7+ requirement on these scripts.

---

_Label `bug` added by @charliermarsh on 2023-12-09 01:21_

---

_Merged by @charliermarsh on 2023-12-09 01:25_

---

_Closed by @charliermarsh on 2023-12-09 01:25_

---

_Branch deleted on 2023-12-09 01:25_

---

_Comment by @konstin on 2023-12-11 08:53_

What is the minimum python version we support? For reference, this is the python version support timeline:

![image](https://github.com/astral-sh/puffin/assets/6826232/1a4409bf-7cc1-44bb-a986-f55cde8c1122)

(https://devguide.python.org/versions/)

---
