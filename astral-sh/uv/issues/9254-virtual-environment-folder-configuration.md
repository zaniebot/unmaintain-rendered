```yaml
number: 9254
title: Virtual Environment Folder Configuration
type: issue
state: closed
author: SDAravind
labels:
  - duplicate
assignees: []
created_at: 2024-11-19T22:13:04Z
updated_at: 2024-11-21T04:28:34Z
url: https://github.com/astral-sh/uv/issues/9254
synced_at: 2026-01-12T15:59:45Z
```

# Virtual Environment Folder Configuration

---

_@SDAravind_

Is it possible to have dedicated virtual environment folder outside project and link it to the project similar to poetry venv folder setup. The reason I ask is, when we switch between windows, linux(wsl) system to work on same project this requires deletion of .venv for each OS and again create virtual environment.

One can use `poetry config` (see below) command to customise virtual environment folder if user needs venv to be in project or default location outside the project that can be customised.

![Screenshot_20241120_033844](https://github.com/user-attachments/assets/807e6674-56b0-47e6-b815-eb8cf4a6cc29)




---

_Comment by @zanieb on 2024-11-19 22:17_

Please see #1495 (and the issues linked there)

---

_Label `duplicate` added by @zanieb on 2024-11-19 22:17_

---

_Closed by @charliermarsh on 2024-11-20 13:31_

---

_Comment by @SDAravind on 2024-11-21 01:11_

Thanks @zanieb.

@charliermarsh - Thanks for creating uv and ruff. They are fantastic tools for the python community. It would be nice to have this specific feature as adoption of uv increases across various platforms and containers.

I hope it would get addressed at some point sooner.

---

_Comment by @zanieb on 2024-11-21 04:28_

Glad you like the tools! We're not sure how best to implement this and we have to balance _many_ requests and fixes. Your use-case is helpful context for prioritization though.

---
