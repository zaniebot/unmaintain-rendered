```yaml
number: 13197
title: UV is unable to install rocm torch if I use a sys_platform marker
type: issue
state: closed
author: TheAyes
labels:
  - documentation
assignees: []
created_at: 2025-04-29T17:20:58Z
updated_at: 2025-04-29T19:13:14Z
url: https://github.com/astral-sh/uv/issues/13197
synced_at: 2026-01-12T16:01:21Z
```

# UV is unable to install rocm torch if I use a sys_platform marker

---

_@TheAyes_

### Summary

I'm currently trying to use uv for it's ease of use but I can't seem to get the platform markers to work correctly.

Here's my pyproject.toml:
```toml
[project]
name = "iona"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "setuptools",
    "wheel",
    "transformers",
    "torch>=2.6.0",
    "torchvision>=0.21.0",
    "torchaudio>=2.6.0",
    "accelerate"
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", marker = "sys_platform != 'linux'" },
    { index = "pytorch-rocm", marker = "sys_platform == 'linux'" },
]
torchvision = [
    { index = "pytorch-rocm", marker = "sys_platform == 'linux'" },
    { index = "pytorch-cpu", marker = "sys_platform != 'linux'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-rocm"
url = "https://download.pytorch.org/whl/rocm6.2"
explicit = true
```

As you can see I'd like to use the rocm version of torch. However this always results in:
```
  × No solution found when resolving dependencies for split (sys_platform == 'linux'):
  ╰─▶ Because only torch{sys_platform == 'linux'}<=2.5.1+rocm6.2 is available and your project depends on torch{sys_platform == 'linux'}>=2.6.0, we can conclude that your project's requirements are unsatisfiable.
```

Even If I decide to downgrade to 2.5.1 it suddenly becomes:
```
× No solution found when resolving dependencies for split (sys_platform == 'linux'):
  ╰─▶ Because there is no version of pytorch-triton-rocm{python_full_version < '3.13' and platform_machine == 'x86_64' and sys_platform == 'linux'}==3.1.0 and torch==2.5.1+rocm6.2 depends on
      pytorch-triton-rocm{python_full_version < '3.13' and platform_machine == 'x86_64' and sys_platform == 'linux'}==3.1.0, we can conclude that torch==2.5.1+rocm6.2 cannot be used.
      And because only the following versions of torch{sys_platform == 'linux'} are available:
          torch{sys_platform == 'linux'}<2.5.1
          torch{sys_platform == 'linux'}==2.5.1+rocm6.2
      and your project depends on torch{sys_platform == 'linux'}>=2.5.1, we can conclude that your project's requirements are unsatisfiable.
```

So the only workaround I know of right now is removing the environment marker which resolves the issue but will install the wrong index unless I change it manually each time.

### Platform

NixOS 25.05.20250423.8a2f738 (Warbler) x86_64

### Version

uv 0.6.16

### Python version

3.13 (can't check cause of  the dependency issue)

---

_Label `bug` added by @TheAyes on 2025-04-29 17:20_

---

_Comment by @charliermarsh on 2025-04-29 17:24_

The first error looks correct: there is no `torch>=2.6.0` on the ROCm6.2 index, so that's impossible to resolve.

The second issue looks correct too. You need to tell uv to look for `pytorch-triton-rocm` on the PyTorch index, probably something like this:

```toml
[project]
name = "iona"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "setuptools",
    "wheel",
    "transformers",
    "torch>=2.5.1",
    "torchvision>=0.20.1",
    "torchaudio>=2.5.1",
    "accelerate",
    "pytorch-triton-rocm ; sys_platform == 'linux'",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", marker = "sys_platform != 'linux'" },
    { index = "pytorch-rocm", marker = "sys_platform == 'linux'" },
]
torchvision = [
    { index = "pytorch-rocm", marker = "sys_platform == 'linux'" },
    { index = "pytorch-cpu", marker = "sys_platform != 'linux'" },
]
pytorch-triton-rocm = { index = "pytorch-rocm" }

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-rocm"
url = "https://download.pytorch.org/whl/rocm6.2"
explicit = true
```

---

_Comment by @TheAyes on 2025-04-29 17:26_

Wouldn't that mean the docs are incorrect?

https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-environment-markers

P.S.: I tried that. Bt the error remains the same:
```
  × No solution found when resolving dependencies for split (sys_platform == 'linux'):
  ╰─▶ Because only torch{sys_platform == 'linux'}<=2.5.1+rocm6.2 is available and your project depends on torch{sys_platform == 'linux'}>=2.6.0, we can conclude that your project's requirements are unsatisfiable.
```

Will I need remove torch/torchvision from the dependencies for this?

---

_Comment by @charliermarsh on 2025-04-29 17:34_

You also need to adjust the `torch` bound like I mentioned -- I've updated my comment.

---

_Comment by @TheAyes on 2025-04-29 17:41_

Alright this fixed it. Do you mind explaining what I will need to look for in order to resolve issues like this in the future? I'd take this as an opportunity to learn. :)

---

_Comment by @charliermarsh on 2025-04-29 17:45_

Yeah sure. I should also add this example to the docs, similar to the Intel GPU example just below where you linked. Basically, once I saw:

```
Because only torch{sys_platform == 'linux'}<=2.5.1+rocm6.2 is available and your project depends on torch{sys_platform == 'linux'}>=2.6.0, we can conclude that your project's requirements are unsatisfiable.
```

I opened up the ROCm index (https://download.pytorch.org/whl/rocm6.2/torch) and saw that the most recent supported version was `2.5.1` based on the filenames (e.g., `torch-2.5.1+rocm6.2-cp39-cp39-linux_x86_64.whl`). So I knew we needed to lower the version requirement (which the error message hints at).

Then when I saw:

```
Because there is no version of pytorch-triton-rocm{python_full_version < '3.13' and platform_machine == 'x86_64' and sys_platform == 'linux'}==3.1.0 and torch==2.5.1+rocm6.2 depends on
      pytorch-triton-rocm{python_full_version < '3.13' an...
```

I had a suspicion that this package was only available on the ROCm index, since the error is saying it doesn't exist. So then I suspected that we had to mark it as "coming from the ROCm index" with `tool.uv.sources`.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-29 17:45_

---

_Label `bug` removed by @charliermarsh on 2025-04-29 17:45_

---

_Label `documentation` added by @charliermarsh on 2025-04-29 17:45_

---

_Closed by @TheAyes on 2025-04-29 17:46_

---

_Comment by @charliermarsh on 2025-04-29 17:47_

Confusingly I'm gonna keep this open so that I remember to update the documentation.

---

_Reopened by @charliermarsh on 2025-04-29 17:47_

---

_Closed by @charliermarsh on 2025-04-29 19:13_

---
