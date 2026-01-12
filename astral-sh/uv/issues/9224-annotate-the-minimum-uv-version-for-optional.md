```yaml
number: 9224
title: Annotate the minimum UV version for optional dependencies in PyTorch documentation.
type: issue
state: closed
author: FishAlchemist
labels:
  - documentation
assignees: []
created_at: 2024-11-19T13:05:04Z
updated_at: 2024-11-19T21:21:44Z
url: https://github.com/astral-sh/uv/issues/9224
synced_at: 2026-01-12T15:59:45Z
```

# Annotate the minimum UV version for optional dependencies in PyTorch documentation.

---

_@FishAlchemist_

Since this feature is only supported in the new version, there will be errors for the old version, and users may be unsure of what to do. Therefore, should we include version notes in the documentation before the release?

----------
https://github.com/astral-sh/uv/blob/main/docs/guides/integration/pytorch.md#configuring-accelerators-with-optional-dependencies
![image](https://github.com/user-attachments/assets/7c094879-55da-4635-bf95-cf1fbd2420e1)

```toml
[tool.uv.sources]
torch = [
{ index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
{ index = "pytorch-cu124", extra = "cu124" },
]
```
uv 0.5.0 (8d665267c 2024-11-07)
![image](https://github.com/user-attachments/assets/84a6f289-6320-4787-a02b-3291664e602f)
uv 0.5.2 
* https://github.com/astral-sh/uv/issues/9221


---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-19 19:46_

---

_Label `documentation` added by @charliermarsh on 2024-11-19 21:11_

---

_Closed by @zanieb on 2024-11-19 21:21_

---
