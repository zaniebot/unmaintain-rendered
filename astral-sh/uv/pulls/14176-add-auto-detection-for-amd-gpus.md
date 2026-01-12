```yaml
number: 14176
title: Add auto-detection for AMD GPUs
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/amd
created_at: 2025-06-21T01:59:16Z
updated_at: 2025-06-21T15:21:07Z
url: https://github.com/astral-sh/uv/pull/14176
synced_at: 2026-01-12T16:11:04Z
```

# Add auto-detection for AMD GPUs

---

_@charliermarsh_

## Summary

Allows `--torch-backend=auto` to detect AMD GPUs. The approach is fairly well-documented inline, but I opted for `rocm_agent_enumerator` over (e.g.) `rocminfo` since it seems to be the recommended approach for scripting: https://rocm.docs.amd.com/projects/rocminfo/en/latest/how-to/use-rocm-agent-enumerator.html.

Closes https://github.com/astral-sh/uv/issues/14086.

## Test Plan

```
root@rocm-jupyter-gpu-mi300x1-192gb-devcloud-atl1:~# ./uv-linux-libc-11fb582c5c046bae09766ceddd276dcc5bb41218/uv pip install torch --torch-backend=auto
Resolved 11 packages in 251ms
Prepared 2 packages in 6ms
Installed 11 packages in 257ms
 + filelock==3.18.0
 + fsspec==2025.5.1
 + jinja2==3.1.6
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.5
 + pytorch-triton-rocm==3.3.1
 + setuptools==80.9.0
 + sympy==1.14.0
 + torch==2.7.1+rocm6.3
 + typing-extensions==4.14.0
```


---

_Label `enhancement` added by @charliermarsh on 2025-06-21 01:59_

---

_Review requested from @geofft by @charliermarsh on 2025-06-21 02:34_

---

_@zanieb reviewed on 2025-06-21 13:20_

---

_Review comment by @zanieb on `docs/guides/integration/pytorch.md`:447 on 2025-06-21 13:20_

Perhaps, now that you have another "and"
```suggestion
When enabled, uv will query for the installed CUDA driver and AMD GPU versions then use the
```

---

_@zanieb approved on 2025-06-21 13:20_

---

_Merged by @charliermarsh on 2025-06-21 15:21_

---

_Closed by @charliermarsh on 2025-06-21 15:21_

---

_Branch deleted on 2025-06-21 15:21_

---
