---
number: 8861
title: "support for \"uv config --list\""
type: issue
state: closed
author: woutervh
labels:
  - duplicate
assignees: []
created_at: 2024-11-06T11:16:47Z
updated_at: 2024-11-06T15:40:48Z
url: https://github.com/astral-sh/uv/issues/8861
synced_at: 2026-01-07T13:12:18-06:00
---

# support for "uv config --list"

---

_Issue opened by @woutervh on 2024-11-06 11:16_

Using uv 0.4.30

uv can be configured in several ways:

- environment variables
- .env-file
- pyproject.toml 
- ~/.config/uv/uv.toml


I find myself situations where I need to troubleshoot why uv is not seeing the correct index-url.

Currently there is no direct easy way to see which settings uv is actually seeing and where they originate from.

I wish for a "uv config --list"  to display the settings,

equivalent to git:

```
> git config --global --list
user.name=...
...
```

and poetry:
```
> poetry config --list
virtualenvs.create = true
...
```



---

_Comment by @zanieb on 2024-11-06 15:39_

There's `uv run --show-settings` but it's not very user friendly.

---

_Comment by @zanieb on 2024-11-06 15:40_

Going to close this in favor of https://github.com/astral-sh/uv/issues/6042

---

_Closed by @zanieb on 2024-11-06 15:40_

---

_Label `duplicate` added by @zanieb on 2024-11-06 15:40_

---
