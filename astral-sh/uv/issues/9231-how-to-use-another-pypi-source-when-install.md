---
number: 9231
title: How to use another pypi source when install packages
type: issue
state: closed
author: rxy1212
labels: []
assignees: []
created_at: 2024-11-19T15:49:41Z
updated_at: 2024-11-20T06:19:13Z
url: https://github.com/astral-sh/uv/issues/9231
synced_at: 2026-01-10T01:24:38Z
---

# How to use another pypi source when install packages

---

_Issue opened by @rxy1212 on 2024-11-19 15:49_

How to use another pypi source when install packages just like "pip install -i" do.


---

_Comment by @FishAlchemist on 2024-11-19 15:51_

``uv pip install -i`` ?
```
 -i, --index-url <INDEX_URL>                (Deprecated: use `--default-index` instead) The URL of the Python package index (by default:
                                             <https://pypi.org/simple>) [env: UV_INDEX_URL=]
```

Just specify the other source. If you only need package from that source and have no other requirements, that's all you need.

---

_Comment by @rxy1212 on 2024-11-20 06:19_

@FishAlchemist Thank you. That's make sense.

---

_Closed by @rxy1212 on 2024-11-20 06:19_

---
