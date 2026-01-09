---
number: 15429
title: python-platfrom is not used when resolving pylock.toml
type: issue
state: closed
author: ZzEeKkAa
labels:
  - bug
assignees: []
created_at: 2025-08-21T19:49:18Z
updated_at: 2025-08-21T19:52:40Z
url: https://github.com/astral-sh/uv/issues/15429
synced_at: 2026-01-07T13:12:19-06:00
---

# python-platfrom is not used when resolving pylock.toml

---

_Issue opened by @ZzEeKkAa on 2025-08-21 19:49_

### Summary

If I run `uv pip compile` with `--python-platform` specified the output pylock.toml contains wheels for every python version and every platform possible. I want to specify resolution scope only for specific python version and target platform.

```
uv pip compile --python-platform x86_64-manylinux_2_28 requirements.in -o pylock.toml
```


```#requirements.in
cffi==1.17.1
```



### Platform

Ubuntu 20.04 x86_64

### Version

uv 0.8.12

### Python version

Python 3.11.13

---

_Label `bug` added by @ZzEeKkAa on 2025-08-21 19:49_

---

_Comment by @charliermarsh on 2025-08-21 19:52_

Thanks. I think this is the same as #13413.

---

_Closed by @charliermarsh on 2025-08-21 19:52_

---
