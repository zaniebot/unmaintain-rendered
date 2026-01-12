```yaml
number: 14289
title: Cannot specify multiple uv.sources which depend on platform markers
type: issue
state: open
author: adstalla
labels:
  - enhancement
assignees: []
created_at: 2025-06-26T18:05:52Z
updated_at: 2025-06-26T19:14:51Z
url: https://github.com/astral-sh/uv/issues/14289
synced_at: 2026-01-12T16:01:46Z
```

# Cannot specify multiple uv.sources which depend on platform markers

---

_@adstalla_

### Summary

Hi all, I'm trying to build an application using pytorch which will be distributed to multiple platforms.
I'm trying to write my pyproject.toml so that it /just works/ for my team, where we develop and deploy to both x86 and aarch64.

I have found some prebuilt Jetson Orin wheels from dustynv over at https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048

I have the following in my pyproject.toml:

```toml
[project]
dependencies = [
    "torch>=2.3.0",
    "torchvision>=0.20.0",
]

[tool.uv]
environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
    "sys_platform == 'linux' and platform_machine == 'aarch64'",
]
override-dependencies = [
    "torch==2.3.0; platform_machine == 'aarch64'",
    "torchvision>=0.18.0; platform_machine == 'aarch64'",
]

[[tool.uv.index]]
name = "pytorch-stable-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true

[tool.uv.sources]
torch = { path = "whl/torch-2.3.0-cp310-cp310-linux_aarch64.whl", marker = "sys_platform == 'aarch64'" }
torchvision = { path = "whl/torchvision-0.18.0a0+6043bc2-cp310-cp310-linux_aarch64.whl", marker = "sys_platform == 'aarch64'" }
torch = { index = "pytorch-stable-cu128", marker = "sys_platform == 'x86_64'" }
torchvision = { index = "pytorch-stable-cu128", marker = "sys_platform == 'x86_64'" }
```


The gist of what I'm trying to do here is:
- make uv use pytorch cu128 repository for x86
- make uv use my local prebuilt wheel for aarch64


The problem with this toml of course is that I have duplicate keys in `tool.uv.sources`. For now I am mostly OK just leaving out the x86 declaration, but I'm imaging at some point I will want to add cpu only support for say `darwin` platform, etc. I feel like I'm not seeing a proper mechanism to leverage platform markers when specifying my build sources.

Could I get some guidance on whether I am using uv the way its intended here, and if there is a common strategy others are using to solve multi-arch pyproject configuration?

### Example

_No response_

---

_Label `enhancement` added by @adstalla on 2025-06-26 18:05_

---

_Comment by @konstin on 2025-06-26 18:13_

Can you use the `whl` directory as a flat index (https://docs.astral.sh/uv/concepts/indexes/#flat-indexes) and use (conflicting) indexes?

---

_Comment by @adstalla on 2025-06-26 19:14_

Cool I hadn't seen that feature. So if I understand correctly, this treats my whl/ directory an index. As long as my wheel file name informs uv that it supports aarch64, other platforms will fall back to the default index.

How would I structure my pyproject to inform uv to prefer the whl/ index first, the pytorch-stable-cu128 repository 2nd, and finally PyPi as the default fallback? I think I'm still stuck on needing multiple source targets by platform markers.

I suppose I could just copy wheels from pytorch-stable-cu128 and just stick to whl/ + PyPi only.

---
