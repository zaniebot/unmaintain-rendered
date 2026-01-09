---
number: 9647
title: "`uv sync`: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform"
type: issue
state: open
author: jfaust
labels:
  - bug
  - resolver
  - needs-design
assignees: []
created_at: 2024-12-04T20:13:16Z
updated_at: 2025-02-07T12:17:32Z
url: https://github.com/astral-sh/uv/issues/9647
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv sync`: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

---

_Issue opened by @jfaust on 2024-12-04 20:13_

Similar to #8922, but with `uv sync`. With the following pyproject.toml:
```
[project]
name = "uv-repro"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = ["jinja2==3.1.4"]

[tool.uv]
index-strategy = 'unsafe-best-match'
find-links = ['https://download.pytorch.org/whl/torch_stable.html']
extra-index-url = [
    'https://download.pytorch.org/whl/cpu',
]
```

I get:
```
Resolved 3 packages in 311ms
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

If I add `https://pypi.org/simple` to the beginning of the `extra-index-url` list, the problem goes away.

---

_Comment by @zanieb on 2024-12-04 20:17_

To confirm, I can reproduce this

```
❯ uv lock
Resolved 3 packages in 952ms
Removed anyio v4.6.2.post1
Removed certifi v2024.8.30
Removed example v0.1.0
Removed h11 v0.14.0
Removed httpcore v1.0.7
Removed httpx v0.28.0
Removed idna v3.10
Added jinja2 v3.1.4
Added markupsafe v3.0.2
Removed sniffio v1.3.1
Added uv-repro v0.1.0
❯ uv sync
Resolved 3 packages in 407ms
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

The lock entry is

```toml
[[package]]
name = "markupsafe"
version = "3.0.2"
source = { registry = "https://download.pytorch.org/whl/cpu" }
wheels = [
    { url = "https://download.pytorch.org/whl/MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:15ab75ef81add55874e7ab7055e9c397312385bd9ced94920f2802310c930396" },
]
```

---

_Label `resolver` added by @zanieb on 2024-12-04 20:19_

---

_Comment by @zanieb on 2024-12-04 20:21_

@charliermarsh looks like the universal resolver to account for wheels on additional indexes?

---

_Label `bug` added by @zanieb on 2024-12-04 20:21_

---

_Comment by @zanieb on 2024-12-04 20:26_

@jfaust I think the recommendation here would be to use the new `index` APIs and be explicit about where you want your packages to come from.

https://docs.astral.sh/uv/guides/integration/pytorch/
https://docs.astral.sh/uv/configuration/indexes/

---

_Label `needs-design` added by @zanieb on 2024-12-04 21:11_

---

_Comment by @jfaust on 2024-12-04 22:12_

@zanieb Got it, that's much nicer. I'm brand new to `uv` so was just naively trying to convert a `requirements.txt`. In the repro it works great, I'll need to check a more involved example.

Should I close this, or would you still expect the original to have worked?

---

_Comment by @zanieb on 2024-12-04 22:15_

We should at least have a better error message for the original.

---

_Referenced in [astral-sh/uv#9711](../../astral-sh/uv/issues/9711.md) on 2024-12-07 20:47_

---

_Comment by @charliermarsh on 2024-12-14 01:56_

I'm starting to think about this problem.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-15 03:10_

---

_Referenced in [astral-sh/uv#9928](../../astral-sh/uv/pulls/9928.md) on 2024-12-16 03:23_

---

_Referenced in [astral-sh/uv#10237](../../astral-sh/uv/issues/10237.md) on 2024-12-30 15:29_

---

_Referenced in [replit/upm#323](../../replit/upm/pulls/323.md) on 2024-12-31 19:37_

---

_Comment by @joellidin on 2025-01-10 10:07_

Hello, I am also having a similar problem. When following the [guide](https://docs.astral.sh/uv/guides/integration/pytorch/#using-a-pytorch-index) on how to use `uv` with pytorch, I get the following error when trying to upgrade my dependencies with `uv sync -U`:

```bash
error: Distribution `torch==2.5.1 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

I am pretty sure this worked before so I don't really know what have changed. I have also tried to set a marker specifically for linux but still get the issue.

```toml
[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", marker = "platform_system == 'Linux'" },
]
torchvision = [
  { index = "pytorch-cpu", marker = "platform_system == 'Linux'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
``` 

---

_Referenced in [astral-sh/uv#10520](../../astral-sh/uv/issues/10520.md) on 2025-01-11 17:08_

---

_Comment by @haruka0000 on 2025-01-29 16:19_

Hello, I also encountered the same issue. I would be grateful if measures could be taken.

For now, I was able to resolve the issue by fixing it to "markupsafe==2.1.3" as shown in the following file, so I will share it.

```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "numpy>=2.1.2",
    "torch>=2.5.1",
]


[project.optional-dependencies]
rocm = [
    "torch>=2.1.0",
    "torchvision>=0.20.0",
    "markupsafe==2.1.3",
]


[tool.uv.sources]
torch = [
    { index = "torch-rocm", extra = "rocm" },
]

[[tool.uv.index]]
name = "torch-rocm"
url = "https://download.pytorch.org/whl/rocm6.2"
```

---

_Comment by @lbsucceed on 2025-02-03 13:25_

> Hello, I also encountered the same issue. I would be grateful if measures could be taken.
> 
> For now, I was able to resolve the issue by fixing it to "markupsafe==2.1.3" as shown in the following file, so I will share it.
> 
> [project]
> name = "test"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> requires-python = ">=3.12"
> dependencies = [
>     "numpy>=2.1.2",
>     "torch>=2.5.1",
> ]
> 
> 
> [project.optional-dependencies]
> rocm = [
>     "torch>=2.1.0",
>     "torchvision>=0.20.0",
>     "markupsafe==2.1.3",
> ]
> 
> 
> [tool.uv.sources]
> torch = [
>     { index = "torch-rocm", extra = "rocm" },
> ]
> 
> [[tool.uv.index]]
> name = "torch-rocm"
> url = "https://download.pytorch.org/whl/rocm6.2"

Yes, when I tried downgrading to markupsafe==2.1.5 and markupsafe==2.1.3, the error disappeared.
```shell
(test) root@DESKTOP:~/test# uv add torch==2.5.0 torchvision==0.20.0 torchaudio==2.5.0 --extra-index-url https://download.pytorch.org/whl/cpu
warning: Indexes specified via `--extra-index-url` will not be persisted to the `pyproject.toml` file; use `--index` instead.
Resolved 18 packages in 2.67s
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.11 (`cp311`), but `markupsafe` (v3.0.2) only has wheels with the following Python implementation tag: `cp313`
(test) root@DESKTOP:~/test# uv add markupsafe==2.1.3
Resolved 2 packages in 119ms
Prepared 1 package in 152ms
Uninstalled 1 package in 0.90ms
Installed 1 package in 25ms
 - markupsafe==2.1.5
 + markupsafe==2.1.3
(test) root@DESKTOP:~/test# uv add torch==2.5.0 torchvision==0.20.0 torchaudio==2.5.0 --extra-index-url https://download.pytorch.org/whl/cpu
warning: Indexes specified via `--extra-index-url` will not be persisted to the `pyproject.toml` file; use `--index` instead.
Resolved 18 packages in 2.58s
Audited 13 packages in 0.01ms

---

_Comment by @Chiggy-Playz on 2025-02-07 12:12_

**Edit: Seems like I've misunderstood the current issue's topic and posted something about a different issue. But I still think this could be added as a new feature to make adding explicit indexes easier. Sorry 😓**

I've noticed something and maybe there could a solution to this problem without downgrading.
All commands run in a new project built by `uv init`

I've noticed when i run `uv add`, pyproject.toml contains the following:

```toml
[project]
name = "test3"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "torch",
    "torchvision",
]

[tool.uv.sources]
torch = { index = "pytorch-cu124" }
torchvision = { index = "pytorch-cu124" }

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
```

And after which i get the error and hence the content is rolled back and dependencies and indexes are removed. 
Now if we [compare this to the example](https://docs.astral.sh/uv/guides/integration/pytorch/) pyproject given in toml to install pytorch, **the only difference** is that the pytorch source is not _explicit_. 

So, what I did was paste the above content into the pyproject manually, add `explicit=true` to the index, delete `uv.lock` if it exists, and then run `uv sync` again. And then it worked perfectly fine! 
```
PS D:\VirtualEnvironments\test3> uv sync
Resolved 28 packages in 3.40s
Installed 12 packages in 2.01s
 + filelock==3.17.0
 + fsspec==2025.2.0
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.4.2
 + numpy==2.2.2
 + pillow==11.1.0
 + sympy==1.13.1
 + torch==2.6.0+cu124
 + torchvision==0.21.0+cu124
 + typing-extensions==4.12.2
```

Proposal: Add a `--explicit` flag or maybe `--explicit-index` which will set the index as explicit? 

---

_Referenced in [Avasam/typed-D3DShot#5](../../Avasam/typed-D3DShot/pulls/5.md) on 2025-10-19 16:13_

---
