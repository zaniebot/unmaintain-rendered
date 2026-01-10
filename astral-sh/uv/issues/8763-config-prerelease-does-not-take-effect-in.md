---
number: 8763
title: config prerelease does not take effect in pyproject.toml
type: issue
state: closed
author: eruvanos
labels:
  - question
assignees: []
created_at: 2024-11-02T13:10:11Z
updated_at: 2024-11-02T19:40:37Z
url: https://github.com/astral-sh/uv/issues/8763
synced_at: 2026-01-10T01:24:32Z
---

# config prerelease does not take effect in pyproject.toml

---

_Issue opened by @eruvanos on 2024-11-02 13:10_

uv: 0.4.29

adding the config:
```
[tool.uv.pip]
prerelease = "allow"
```

does not take any effect, but the following does:

```
[tool.uv]
prerelease = "allow"
```

> Not sure which one is intended, but the documentation is not correct at the moment: [Link](https://docs.astral.sh/uv/reference/settings/#pip_prerelease)

---

_Comment by @zanieb on 2024-11-02 14:00_

Please see https://docs.astral.sh/uv/configuration/files/#configuring-the-pip-interface

---

_Label `question` added by @zanieb on 2024-11-02 14:00_

---

_Comment by @eruvanos on 2024-11-02 19:40_

I see, my fault. Thank you

---

_Closed by @eruvanos on 2024-11-02 19:40_

---
