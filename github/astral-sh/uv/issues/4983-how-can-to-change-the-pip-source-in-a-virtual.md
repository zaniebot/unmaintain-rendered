---
number: 4983
title: how can to change the pip source in a virtual environment, use the following command
type: issue
state: closed
author: dividduang
labels:
  - question
assignees: []
created_at: 2024-07-11T01:20:44Z
updated_at: 2024-07-12T06:23:49Z
url: https://github.com/astral-sh/uv/issues/4983
synced_at: 2026-01-07T13:12:17-06:00
---

# how can to change the pip source in a virtual environment, use the following command

---

_Issue opened by @dividduang on 2024-07-11 01:20_

```
python --version
Python 3.8.19

uv --version
uv 0.2.20

os--windows
```
```
C:\Users>uv pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
error: unrecognized subcommand 'config'
```

thanks ! 

---

_Comment by @charliermarsh on 2024-07-12 02:37_

We don't have a `config` command right now. But you could add this to a `uv.toml` in that directory:
```toml
[pip]
index-url = "https://pypi.tuna.tsinghua.edu.cn/simple"
```

---

_Comment by @charliermarsh on 2024-07-12 02:37_

Happy to answer follow-ups!

---

_Closed by @charliermarsh on 2024-07-12 02:37_

---

_Label `question` added by @charliermarsh on 2024-07-12 02:38_

---

_Comment by @dividduang on 2024-07-12 06:23_

❤️❤️❤️❤️❤️❤️❤️❤️❤️❤️❤️❤️

---
