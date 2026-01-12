```yaml
number: 2529
title: Allow creating a virtual environment in a folder with existing files
type: issue
state: closed
author: gaborbernat
labels:
  - enhancement
assignees: []
created_at: 2024-03-18T23:23:58Z
updated_at: 2024-05-01T16:34:53Z
url: https://github.com/astral-sh/uv/issues/2529
synced_at: 2026-01-12T15:58:38Z
```

# Allow creating a virtual environment in a folder with existing files

---

_@gaborbernat_

```
❯ lsd --tree .venv/
 .venv
└──  magic.txt
~/git/github/tox-uv on  main [!?⇕]
❯ uv venv
Using Python 3.12.1 interpreter at: /Users/bgabor8/.pyenv/versions/3.12.1/bin/python3
Creating virtualenv at: .venv
uv::venv::creation

  × Failed to create virtualenv
  ╰─▶ The directory `.venv` exists, but it's not a virtualenv
```

`tox` likes to put a few other files (`tmp` folder, `.tox-info.json`, etc) in this folder that breaks `uv venv`....

Is fine if is hidden behind a `--allow-extra-content` flag.

---

_Comment by @zanieb on 2024-03-18 23:29_

I wonder if this should just be gated by a simple `--force` flag.

---

_Label `enhancement` added by @zanieb on 2024-03-18 23:29_

---

_Comment by @gaborbernat on 2024-03-19 01:59_

Works for me 😀

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-19 17:24_

---

_Closed by @charliermarsh on 2024-05-01 16:34_

---
