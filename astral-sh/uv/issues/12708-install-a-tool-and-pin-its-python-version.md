```yaml
number: 12708
title: install a tool and pin its Python version
type: issue
state: open
author: jabbalaci
labels:
  - question
assignees: []
created_at: 2025-04-07T06:30:12Z
updated_at: 2025-04-07T06:30:12Z
url: https://github.com/astral-sh/uv/issues/12708
synced_at: 2026-01-12T16:01:10Z
```

# install a tool and pin its Python version

---

_@jabbalaci_

### Question

There's a tool that only works with Python 3.11 (and not with later versions), so I need to install it like this:

    uv tool install pynt --python=3.11

It works well. And I have an upgrade script that I execute once in a while that contains this line:

    uv tool upgrade --all

However, it upgrades the Python version of pynt to Python 3.13, but pynt is not compatible with that.

What I want: the Python version used for pynt should be pinned to Python 3.11 . With "uv tool upgrade --all" I want to install newer versions of the tools. If there's no newer version of pynt, it should be left alone. How to do this? Thanks.

### Platform

Linux 6.14.0-1-MANJARO x86_64 GNU/Linux

### Version

uv 0.6.12

---

_Label `question` added by @jabbalaci on 2025-04-07 06:30_

---
