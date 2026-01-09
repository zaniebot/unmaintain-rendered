---
number: 7202
title: Issues creating a cuda-enabled pytorch environment with UV
type: issue
state: closed
author: pstjohn
labels: []
assignees: []
created_at: 2024-09-08T23:14:24Z
updated_at: 2025-07-10T10:13:22Z
url: https://github.com/astral-sh/uv/issues/7202
synced_at: 2026-01-07T13:12:17-06:00
---

# Issues creating a cuda-enabled pytorch environment with UV

---

_Issue opened by @pstjohn on 2024-09-08 23:14_

I'm having some issues using `uv lock` & `uv sync` in a package that needs to use cuda-enabled pytorch wheels.

Here's a minimal-ish reproduction:

https://github.com/pstjohn/uv-torchvision-repro
The workspace stuff is there since I'd like to eventually get this working in a monorepo-like environment (#6935)

The desired behavior would be that `uv lock` would match what I get with 
`pip install --extra-index-url https://download.pytorch.org/whl/cu124 torch torchvision`, which would be (at the time of writing) to install torch 2.4.1 and torchvision 0.19.1 both in their linux_x86_64 / cu124 / cp310 variants.

But instead I get the following error: 
```
0.197 error: distribution torchvision==0.15.0 @ registry+https://download.pytorch.org/whl/cu124 
can't be installed because it doesn't have a source distribution or wheel for the current platform
```

If I use `uv pip install --extra-index-url https://download.pytorch.org/whl/cu124 torch torchvision`, I get `torch==2.4.1+cu124` and `torchvision==0.2.0`. (i.e., seemingly not pulling torchvision from the cu124 index).

Any thoughts on where I might be going wrong? Thanks!

---

_Comment by @pstjohn on 2024-09-09 14:41_

Just some additional data:

with `torchvision == 0.19.1`, 
```
$ uv lock --extra-index-url https://download.pytorch.org/whl/cu124
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torchvision==0.19.1 and subpackage-b depends on torchvision==0.19.1, we can conclude that subpackage-b's
      requirements are unsatisfiable.
      And because your workspace requires subpackage-b, we can conclude that your workspace's requirements are unsatisfiable.
```

but with `torchvision == 0.19.1+cu124`, it resolves correctly.

---

_Comment by @charliermarsh on 2024-09-09 14:58_

Have you taken a look at https://docs.astral.sh/uv/pip/compatibility/#local-version-identifiers?

---

_Comment by @pstjohn on 2024-09-09 15:34_

Yeah that seems to explain it. It's odd that specifying `torchvision==0.19.1` _doesn't_ accept `0.19.1+cu124`, though.

Having to pin the versions exactly to get cuda-enabled wheels isn't great for us. As a library we'd just like to depend on `torch`, but in setting up a python environment we'll need to be able to install the cuda-compiled dependency versions (which you typically do for different cuda versions just by changing the `index-url`, https://pytorch.org/get-started/locally/#start-locally)

I'm wondering if an option to `--ignore-local-version-identifiers` in `uv lock` would be enough to get this working? But there might be edge cases to those local identifiers I'm not following

---

_Comment by @msarahan on 2024-09-09 16:09_

If I'm reading the docs right, it's that `torchvision==0.19.1` doesn't match `0.19.1+cu124` because the whole spec is ignored, due to the fact that it has a local version identifier.

This general problem is something that I would like to solve by improving metadata. That's quite a rabbit hole, but if you'd like to learn more:

* https://discuss.python.org/t/what-to-do-about-gpus-and-the-built-distributions-that-support-them/7125/64 - Rehash just before PyCon 2024
* https://discuss.python.org/t/implementation-variants-rehashing-and-refocusing/54884 - Rehash after a long discussion with Oscar Benjamin and others
* https://discuss.python.org/t/selecting-variant-wheels-according-to-a-semi-static-specification/53446/111 - my latest proposal, which has side-tracked me to [a PEP for index priority](https://docs.google.com/document/d/16T0enLLEoKP6MvlTWsvPdXmCDXsWHANV5X1YPxjuoBw/edit?usp=sharing). @charliermarsh you may be interested in this. It is deeply influenced by uv's design.

---

_Comment by @vajranaga on 2024-09-12 01:13_

If trying to add cuda-enabled pytorch to a project, I found the following process worked for me (Windows 11, but should work in Linux):

1. Install torch into the venv first with: `> uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124` in the project directory. This should be done first before other packages, as it will search the pytorch index for the related dependencies and will fail, likely, like this:
```
uv add torch --index-url https://download.pytorch.org/whl/cu124
  × No solution found when resolving dependencies:
  ╰─▶ Because jupyterlab was not found in the package registry and your project depends on jupyterlab>=4.2.5, we can
      conclude that your project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
```

2. Add torch to the project dependencies with `> uv add torch torchvision torchaudio`.
When I tried this first (before installing with uv pip install) I got the following error: 
```
uv add torch --index-url https://download.pytorch.org/whl/cu124
Resolved 11 packages in 597ms
error: distribution torch==2.4.1 @ registry+https://download.pytorch.org/whl/cu124 can't be installed because it doesn't have a source distribution or wheel for the current platform
```
3. Add remainder of dependencies as required (jupyterlab, notebook, pandas, matplotlib, etc)...

This is what worked for me today. Verified with
```
Python 3.12.6 (tags/v3.12.6:a4a2d2b, Sep  6 2024, 20:11:23) [MSC v.1940 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.cuda.is_available()
True
>>> exit()
```

---

_Comment by @charliermarsh on 2024-09-16 02:51_

> https://discuss.python.org/t/selecting-variant-wheels-according-to-a-semi-static-specification/53446/111 - my latest proposal, which has side-tracked me to [a PEP for index priority](https://docs.google.com/document/d/16T0enLLEoKP6MvlTWsvPdXmCDXsWHANV5X1YPxjuoBw/edit?usp=sharing). @charliermarsh you may be interested in this. It is deeply influenced by uv's design.

@msarahan -- That's really interesting. Is there anything we can do on our side to support the PEP, or the variant-selection proposal?

---

_Comment by @ngbrown on 2024-09-29 06:32_

@vajranaga:

> 2. Add torch to the project dependencies with `> uv add torch torchvision torchaudio`.

Doing this without any index specifier doesn't appear to actually capture the CUDA related sources into the `uv.lock` file. So this would tell me that it's not reproducible for the next time the environment is setup.

Meanwhile, if I use `uv add torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121` as that step, I get the following error:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.4.1 and torchaudio==2.4.1+cu121 depends on torch==2.4.1, we can conclude
      that torchaudio==2.4.1+cu121 cannot be used.
      And because only the following versions of torchaudio are available:
          torchaudio<2.4.1
          torchaudio==2.4.1+cu121
      and your project depends on torchaudio>=2.4.1, we can conclude that your project's requirements are
      unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip
        locking and syncing.
```

Even though on the previous step I had used the `cu121` path on the `uv pip install` step and the output included:

```
 + torch==2.4.1+cu121
 + torchaudio==2.4.1+cu121
 + torchvision==0.19.1+cu121
```

The following works and it produces what looks like would be a working `uv.lock` file:

```
uv add torch==2.4.1+cu121 torchaudio==2.4.1+cu121 torchvision==0.19.1+cu121 --index-url https://download.pytorch.org/whl/cu121
```

Infact, it looks like it's the only thing that is needed:

```ps1
> py -3.12 -m venv .\.venv\
> .\.venv\Scripts\activate.ps1
> pip install uv
> uv add torch==2.4.1+cu121 torchaudio==2.4.1+cu121 torchvision==0.19.1+cu121 --index-url https://download.pytorch.org/whl/cu121
```

Then delete the `.venv` and do the process over, but use `uv sync` and it works fine pulling source urls from the `uv.lock` file.


---

_Comment by @hey-its-ashu on 2024-10-28 07:41_

Is there a way to add torch, torchaudio and torchvision (with gpu and without gpu) in single pyproject.toml file like we can do in `poetry` for example:

```
[tool.poetry.dependencies]
python = "^3.12"

[tool.poetry.group.cuda]
optional = true

[tool.poetry.group.cuda.dependencies]
torch = { version = "^2.4.1", source = "pytorch-cuda", markers = "extra=='cuda' and extra!='cpu'" }
torchaudio = { version = "^2.4.1", source = "pytorch-cuda", markers = "extra=='cuda' and extra!='cpu'" }
torchvision = { version = "^0.19.1", source = "pytorch-cuda", markers = "extra=='cuda' and extra!='cpu'" }

[tool.poetry.extras]
cpu = ["torch", "torchvision"]
cuda = ["torch", "torchvision"]

[[tool.poetry.source]]
name = "pytorch-cuda"
priority = "explicit"
url = "https://download.pytorch.org/whl/cu118"

[[tool.poetry.source]]
name = "pytorch-cpu"
priority = "explicit"
url = "https://download.pytorch.org/whl/cpu"
```


On a side note, after running below `uv` command, I am unable to add any other python package like, numpy, ultralytics etc.

```
uv add torch==2.4.1+cu121 torchaudio==2.4.1+cu121 torchvision==0.19.1+cu121 --index-url https://download.pytorch.org/whl/cu121
```

error:
```
  × No solution found when resolving dependencies for split (python_full_version == '3.8.*' and platform_machine == 'arm64' and platform_system == 'Darwin' and sys_platform ==
  │ 'win32'):
  ╰─▶ Because there is no version of torch==2.4.1+cu121 and your project depends on torch==2.4.1+cu121, we can conclude that your project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

---

_Comment by @charliermarsh on 2024-10-28 12:27_

No, we don't support resolutions with conflicting extra groups right now, though we're adding first-class support for it in the future.

---

_Comment by @YoniChechik on 2024-10-29 09:03_

I've tried to do a frash project with `uv init` and then adding torch + torchvision with cuda. tried many of the examples here but same errors appeared. Can someone help wit hthe basic commands?

p.s.:
torch is such a popular package. I suggest that the docs will explicitly instruct new users how to install it.

---

_Comment by @zanieb on 2024-10-29 14:23_

@YoniChechik see #6523 

---

_Comment by @vajranaga on 2024-10-29 17:02_

I found that the instructions here:
[https://docs.astral.sh/uv/concepts/dependencies/#index](url)

Worked for my rye (with uv) projects too. I just had to manually add those lines for which cuda/cpu index I was using, then `uv sync` and everything worked fine.

Specifically the `explicit = true` in the `[[tools.uv.index]]` block.


---

_Comment by @extrange on 2024-10-30 01:23_

EDIT: Just realized my `uv.lock` specifies the index URL probably due to previously using it, so what I wrote below is not valid.

Contrary to the original poster, I actually can get Pytorch (with the CUDA 12.4 runtime) working with just `uv add torch torchvision torchaudio`.

```toml
[project]
name = "whisper"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "torch>=2.5.1",
    "torchaudio>=2.5.1",
    "torchvision>=0.20.1",
]
```

```sh
$ python -c 'import torch; print(torch.__version__)'
2.5.1+cu124
```

nvidia-smi for my system:

```sh
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.90.07              Driver Version: 550.90.07      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  Tesla T4                       On  |   00000000:00:1E.0 Off |                    0 |
| N/A   35C    P8              8W /   70W |       1MiB /  15360MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+

```


---

_Comment by @hongjiaherng on 2024-10-30 15:25_

I couldn't make it work with the approach of @extrange. And the solution of @vajranaga is indeed working for me!

> I found that the instructions here: [https://docs.astral.sh/uv/concepts/dependencies/#index](url)
> 
> Worked for my rye (with uv) projects too. I just had to manually add those lines for which cuda/cpu index I was using, then `uv sync` and everything worked fine.
> 
> Specifically the `explicit = true` in the `[[tools.uv.index]]` block.

Here's my `pyproject.toml`. I added these manually then run `uv sync`.
```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "torch==2.5.1+cu124",
    "torchaudio==2.5.1+cu124",
    "torchvision==0.20.1+cu124",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
torch = { index = "pytorch-cu124" }
torchvision = { index = "pytorch-cu124" }
torchaudio = { index = "pytorch-cu124" }

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```



---

_Referenced in [astral-sh/uv#6523](../../astral-sh/uv/pulls/6523.md) on 2024-10-30 20:38_

---

_Comment by @YouJiacheng on 2024-11-02 15:51_

~~I have no idea why this can work.~~
It's expected on Linux
<img width="1195" alt="4" src="https://github.com/user-attachments/assets/9e91da0f-995f-459f-93b8-41e4b08a8f5b">


---

_Comment by @zanieb on 2024-11-02 15:55_

@YouJiacheng it looks like the dependency on numpy is optional? What happens if you `uv add numpy` too?

---

_Comment by @YouJiacheng on 2024-11-02 16:22_

~~@zanieb I knew I can install numpy (and it will be fine), my question is, why `uv add torch` install `torch==2.5.1+cu124` for me.~~
It's expected on Linux.

---

_Comment by @zanieb on 2024-11-02 16:42_

Okay, please explain what you're expecting to see when reporting an issue — I can't just guess it from a screenshot. Please read https://docs.astral.sh/uv/pip/compatibility/#local-version-identifiers

---

_Comment by @YouJiacheng on 2024-11-02 17:55_

solved!
this behavior is expected on Linux.
![image](https://github.com/user-attachments/assets/0acc74a8-11a2-48e7-ac32-791117acd40c)


---

_Comment by @charliermarsh on 2024-12-31 02:20_

I believe the issues here have been solved in the last few releases (local versions, etc.).

---

_Closed by @charliermarsh on 2024-12-31 02:20_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-31 02:20_

---

_Comment by @muchengdong on 2025-01-11 14:03_

@hongjiaherng big nice

---

_Comment by @hongjiaherng on 2025-01-19 09:31_

Hi peeps, update for future people here. The latest documentation provides the way to install pytorch with uv (https://docs.astral.sh/uv/guides/integration/pytorch/)

I would say after years of python package management evolution, UV has finally solved pytorch installation! Thanks to the team for the effort!

---

_Comment by @laoshaw on 2025-03-08 04:59_

> Hi peeps, update for future people here. The latest documentation provides the way to install pytorch with uv (https://docs.astral.sh/uv/guides/integration/pytorch/)
> 
> I would say after years of python package management evolution, UV has finally solved pytorch installation! Thanks to the team for the effort!

except it does not work for me, either I'm doing something wrong or the doc is incorrect:
"To start, consider the following (default) configuration, which would be generated by running uv init --python 3.12 followed by uv add torch torchvision.

In this case, PyTorch would be installed from PyPI, which hosts CPU-only wheels for Windows and macOS, and GPU-accelerated wheels on Linux (targeting CUDA 12.4)"

-- my result is, it's CPU for Linux too even though I had cuda running just fine on my Linux, uv does not install the gpu version at all with 'uv add torch torchvision", I had to modify pyproject.toml as shown on that page to get going.

---

_Comment by @charliermarsh on 2025-03-08 14:13_

Are you on ARM Linux? If you're getting the `torch` Linux wheels from PyPI, those always come with CUDA enabled. They don't ship CPU-only Linux wheels to PyPI.

---

_Comment by @laoshaw on 2025-03-08 15:55_

> Are you on ARM Linux? If you're getting the `torch` Linux wheels from PyPI, those always come with CUDA enabled. They don't ship CPU-only Linux wheels to PyPI.

looks like it's my mistake, tried on a different machine with 24.04 ubuntu x86 and it worked, my previous x86 ubuntu might had cuda installation issues though I'm unsure. in short, the doc is correct. Thanks!

---

_Comment by @eric-gitta-moore on 2025-04-26 05:35_

> [@vajranaga](https://github.com/vajranaga):
> 
> > 2. Add torch to the project dependencies with `> uv add torch torchvision torchaudio`.
> 
> Doing this without any index specifier doesn't appear to actually capture the CUDA related sources into the `uv.lock` file. So this would tell me that it's not reproducible for the next time the environment is setup.
> 
> Meanwhile, if I use `uv add torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121` as that step, I get the following error:
> 
> ```
>   × No solution found when resolving dependencies:
>   ╰─▶ Because there is no version of torch==2.4.1 and torchaudio==2.4.1+cu121 depends on torch==2.4.1, we can conclude
>       that torchaudio==2.4.1+cu121 cannot be used.
>       And because only the following versions of torchaudio are available:
>           torchaudio<2.4.1
>           torchaudio==2.4.1+cu121
>       and your project depends on torchaudio>=2.4.1, we can conclude that your project's requirements are
>       unsatisfiable.
>   help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip
>         locking and syncing.
> ```
> 
> Even though on the previous step I had used the `cu121` path on the `uv pip install` step and the output included:
> 
> ```
>  + torch==2.4.1+cu121
>  + torchaudio==2.4.1+cu121
>  + torchvision==0.19.1+cu121
> ```
> 
> The following works and it produces what looks like would be a working `uv.lock` file:
> 
> ```
> uv add torch==2.4.1+cu121 torchaudio==2.4.1+cu121 torchvision==0.19.1+cu121 --index-url https://download.pytorch.org/whl/cu121
> ```
> 
> Infact, it looks like it's the only thing that is needed:
> 
> > py -3.12 -m venv .\.venv\
> > .\.venv\Scripts\activate.ps1
> > pip install uv
> > uv add torch==2.4.1+cu121 torchaudio==2.4.1+cu121 torchvision==0.19.1+cu121 --index-url https://download.pytorch.org/whl/cu121
> Then delete the `.venv` and do the process over, but use `uv sync` and it works fine pulling source urls from the `uv.lock` file.


I don't understand why UV identified me as macOS. 

`platform_machine == 'arm64' and sys_platform == 'darwin'`

```
YOLOv5-LPRNet-Licence-Recognition on  master [?] is 󰏗 v0.1.0 via  v3.8.20 (YOLOv5-LPRNet-Licence-Recognition) at 13:33:14 
❯ uname -a
Linux DESKTOP-SURJD4A 5.15.167.4-microsoft-standard-WSL2 #1 SMP Tue Nov 5 00:21:55 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
YOLOv5-LPRNet-Licence-Recognition on  master [?] is 󰏗 v0.1.0 via  v3.8.20 (YOLOv5-LPRNet-Licence-Recognition) at 13:33:10 
❯ uv add torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu111
warning: Indexes specified via `--index-url` will not be persisted to the `pyproject.toml` file; use `--default-index` instead.
  × No solution found when resolving dependencies for split (python_full_version == '3.8.*' and platform_machine == 'arm64' and sys_platform == 'darwin'):
  ╰─▶ Because matplotlib was not found in the package registry and your project depends on matplotlib>=3.2.2, we can conclude that your project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
``·

---

_Referenced in [tgm-team/tgm#81](../../tgm-team/tgm/pulls/81.md) on 2025-07-03 01:34_

---

_Comment by @linaMallek on 2025-07-10 10:13_

> Are you on ARM Linux? If you're getting the `torch` Linux wheels from PyPI, those always come with CUDA enabled. They don't ship CPU-only Linux wheels to PyPI.

I am on windows 64 i tried to use torch with cu12.6 by adding to toml file as said in the documentation but i only get the cpu version , beside i tried to install onnxruntime-gpu  for insightface its looks its cant just access my gpu !! is there any way to solve this

---
