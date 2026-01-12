```yaml
number: 1550
title: Allow uv to install global packages with supporting flag
type: issue
state: closed
author: savioxavier
labels:
  - duplicate
assignees: []
created_at: 2024-02-16T23:00:03Z
updated_at: 2024-02-17T00:31:25Z
url: https://github.com/astral-sh/uv/issues/1550
synced_at: 2026-01-12T15:58:29Z
```

# Allow uv to install global packages with supporting flag

---

_@savioxavier_

Right now, I use a ton of different CLI tools written in Python to manage my workflow. I also make sure that I regularly upgrade them with the help of `pip review`. Unfortunately, pip is really slow and takes forever to run, which is why I'm super excited about uv.

Currently, it doesn't seem as if uv allows the installation of global packages, which in this case are CLI tools (it always requires a venv). Doing so could keep my CLI tools easier to manage and upgrade and also, separate from my individual projects.

What I propose is a new flag called `-g|--global` to be added to the `uv pip install` command. 

For example:
```sh
$ uv pip install --global frogmouth # a terminal markdown viewer app
```

In addition (although these could be planned for later): 
- `uv pip upgrade --global --all`
- `uv pip list --global`

---

_Comment by @zanieb on 2024-02-17 00:31_

Thanks for your feedback! Great to hear you're excited about the tool :)

Your requests are being tracked in:

- #1401
- #1419 
- #1526 

---

_Closed by @zanieb on 2024-02-17 00:31_

---

_Label `duplicate` added by @zanieb on 2024-02-17 00:31_

---
