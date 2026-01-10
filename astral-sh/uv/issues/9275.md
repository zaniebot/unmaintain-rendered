```yaml
number: 9275
title: Ignore irrelevant platform-specific packages during resolution
type: issue
state: closed
author: Vinno97
labels:
  - enhancement
assignees: []
created_at: 2024-11-20T14:16:24Z
updated_at: 2024-12-07T01:51:45Z
url: https://github.com/astral-sh/uv/issues/9275
synced_at: 2026-01-10T04:36:20Z
```

# Ignore irrelevant platform-specific packages during resolution

---

_Issue opened by @Vinno97 on 2024-11-20 14:16_

Hi,

I've noticed that `uv`  still tries to resolve packages that are supposed to be irrelevant for the configured environment. I'm working on a server with a private registry where we don't have Darwin packages. When I try to install `ipykernel`, however, I see it still tries to resolve a Darwin-specific package, which then fails as it isn't available in our registry. Specifically, it's about `appnope`, which is configured as `'appnope>=0.1.2;platform_system=="Darwin"'` in ipykernel' s pyproject.toml.(https://github.com/ipython/ipykernel/blob/8c4901d691b1f309da3b80eefad5af13d7418185/pyproject.toml#L32C6-L32C13)

I'm using `uv` 0.5.2 on RHEL.

```toml
[project]
name = "test-project"
version = "0.1.0"
description = "My test project"
readme = "README.md"

dependencies = [
    "ipykernel ; sys_platform == 'linux'"
]

[tool.uv]
environments = [
    "sys_platform == 'linux'"
]
```  

```
$ uv lock -v 2>&1 | grep Darwin
DEBUG Solving split (python_full_version == '3.10'and platform_system == 'Darwin' and sys_platform == 'linux') ...
DEBUG Adding transitive dependency for ipykernel==x.x.x: appnope{platform_system == 'Darwin'}*
DEBUG Adding transitive dependency for ipykernel==x.x.x: appnope{platform_system == 'Darwin'}*
```

I think it might be related to https://github.com/astral-sh/uv/issues/9051#issuecomment-2470334438


---

_Comment by @charliermarsh on 2024-11-20 14:18_

You might want to add something like `platform_system == "Linux"` to `environments`? The system doesn't know that `platform_system == 'Darwin'` and `sys_platform == 'linux'` are mutually exclusive -- there's nothing in the spec that says they _have_ to be, it's just true in practice.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 18:20_

---

_Label `enhancement` added by @charliermarsh on 2024-11-20 20:20_

---

_Comment by @Vinno97 on 2024-11-21 08:13_

Oh that's interesting and, though annoying, quite logical. Using platform_system indeed works and this then isn't a bug. 

---

If you think it's worthwhile, I'd be willing to add a small note to your documentation about this, as it currently uses `sys_platform` exclusively. This would then not work if any of your dependencies, like `ipykernel` uses `platform_system` instead of `sys_platform`.

![image](https://github.com/user-attachments/assets/3de9881a-5928-40fe-b5ea-2ff038003498)

---

_Closed by @charliermarsh on 2024-12-07 01:51_

---

_Closed by @charliermarsh on 2024-12-07 01:51_

---
