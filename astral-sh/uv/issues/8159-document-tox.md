```yaml
number: 8159
title: "document: tox"
type: issue
state: closed
author: amirreza8002
labels: []
assignees: []
created_at: 2024-10-13T16:08:53Z
updated_at: 2024-10-13T19:24:16Z
url: https://github.com/astral-sh/uv/issues/8159
synced_at: 2026-01-12T15:59:21Z
```

# document: tox

---

_@amirreza8002_

something like this should get you going:


```
# tox.ini

[testenv]
...
allowlist_externals =
                uvx
                uv
commands_pre = uv sync --all-extras --dev
commands =
    uvx pytest -v {posargs}
...    
```

---

_Comment by @amirreza8002 on 2024-10-13 19:22_

well actually this seems to break
i'll update if i find a solution

---

_Closed by @amirreza8002 on 2024-10-13 19:24_

---
