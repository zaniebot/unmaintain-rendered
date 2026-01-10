---
number: 12282
title: "uv rejects `flash-attn` wheels with local versions"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-03-18T14:28:07Z
updated_at: 2025-03-18T15:12:42Z
url: https://github.com/astral-sh/uv/issues/12282
synced_at: 2026-01-10T01:25:17Z
---

# uv rejects `flash-attn` wheels with local versions

---

_Issue opened by @charliermarsh on 2025-03-18 14:28_

This seems to have caused some problems with the flash-attn wheels.

```
#pyproject.toml
...

[tool.uv]
environments = ["sys_platform == 'darwin'", "sys_platform == 'linux'"]
constraint-dependencies = ["torch==2.5.1"]

[tool.uv.sources]
flash_attn = [
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp310-cp310-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.10'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp311-cp311-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.11'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp312-cp312-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.12'"},
  { url = "https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu12torch2.5cxx11abiFalse-cp313-cp313-linux_x86_64.whl", marker = "sys_platform == 'linux' and python_version == '3.13'"}
]
```

Now causes `uv sync` to error with
```
error: Failed to parse `uv.lock`
  Caused by: The entry for package `flash-attn` v2.7.3 has wheel `flash_attn-2.7.3+cu12torch2.5cxx11abifalse-cp310-cp310-linux_x86_64.whl` with inconsistent version: v2.7.3+cu12torch2.5cxx11abifalse
  ```
  
  Not sure if there is a better way to configure usage of these wheels.

_Originally posted by @kleinhenz in https://github.com/astral-sh/uv/issues/12235#issuecomment-2733196655_
            

---

_Label `bug` added by @charliermarsh on 2025-03-18 14:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-18 14:34_

---

_Referenced in [astral-sh/uv#12285](../../astral-sh/uv/pulls/12285.md) on 2025-03-18 14:58_

---

_Closed by @charliermarsh on 2025-03-18 15:12_

---

_Closed by @charliermarsh on 2025-03-18 15:12_

---
