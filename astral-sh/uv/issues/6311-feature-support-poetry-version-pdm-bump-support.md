---
number: 6311
title: "feature: support poetry version/pdm bump support"
type: issue
state: closed
author: toppk
labels:
  - duplicate
assignees: []
created_at: 2024-08-21T08:34:00Z
updated_at: 2024-08-21T12:39:47Z
url: https://github.com/astral-sh/uv/issues/6311
synced_at: 2026-01-10T01:23:58Z
---

# feature: support poetry version/pdm bump support

---

_Issue opened by @toppk on 2024-08-21 08:34_

It would be nice if `uv` had native capability to update version information in pyproject.toml.

Both pdm and poetry have this capability, here are the links
https://python-poetry.org/docs/cli/#version 
https://github.com/carstencodes/pdm-bump

The pdm support is through a 3rd party pdm plugin.


for example:
```sh
pdm bump micro # creates 0.1.2 from 0.1.1
poetry version patch # creates 0.1.2 from 0.1.1
```


---

_Comment by @zanieb on 2024-08-21 12:39_

Duplicate of https://github.com/astral-sh/uv/issues/6298

---

_Closed by @zanieb on 2024-08-21 12:39_

---

_Label `duplicate` added by @zanieb on 2024-08-21 12:39_

---
