```yaml
number: 8744
title: 【Bug】 When uvx installed the package, I did not use the index-url I set 
type: issue
state: closed
author: gcil125
labels:
  - question
assignees: []
created_at: 2024-11-01T02:36:06Z
updated_at: 2024-11-01T13:33:06Z
url: https://github.com/astral-sh/uv/issues/8744
synced_at: 2026-01-12T15:59:33Z
```

# 【Bug】 When uvx installed the package, I did not use the index-url I set 

---

_@gcil125_

My uv is installed through pip 




![image](https://github.com/user-attachments/assets/faf60db0-04f6-46d9-a158-453590da9af9)





---

_Comment by @charliermarsh on 2024-11-01 02:40_

I think this should be on the uv repo.

---

_Comment by @charliermarsh on 2024-11-01 13:33_

This is by design. `uvx` doesn't use project-local configuration, since the tools it installs are available globally. You can set `index-url` in your user-level or system-level `uv.toml` file. See: https://docs.astral.sh/uv/configuration/files/.

---

_Label `question` added by @charliermarsh on 2024-11-01 13:33_

---

_Closed by @charliermarsh on 2024-11-01 13:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-01 13:33_

---
