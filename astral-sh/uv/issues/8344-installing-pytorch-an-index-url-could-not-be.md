---
number: 8344
title: "Installing PyTorch: An index URL could not be queried"
type: issue
state: closed
author: Leon0402
labels:
  - bug
assignees: []
created_at: 2024-10-18T18:04:53Z
updated_at: 2025-01-31T18:21:44Z
url: https://github.com/astral-sh/uv/issues/8344
synced_at: 2026-01-10T01:24:27Z
---

# Installing PyTorch: An index URL could not be queried

---

_Issue opened by @Leon0402 on 2024-10-18 18:04_

Setup is: 
```
uv init
uv venv --python 3.10
```

Leading to the simple `pyproject.toml`:
```
[project]
name = "test2"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
```

Trying to install torch with: 
```
uv add torch torchvision torchaudio --default-index https://download.pytorch.org/whl/cu118
```

leads to the error:
```
error: Distribution `torchaudio==2.0.0 @ registry+https://download.pytorch.org/whl/cu118` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

Alternatively using pip: 
```
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

leads to a different error:

```
⠋ lit==15.0.7                                                                                                                                                                     × Failed to download and build `lit==15.0.7`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `setuptools>=40.8.0`, `wheel`
  ╰─▶ Because wheel was not found in the package registry and you require wheel, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://download.pytorch.org/whl/cu118) could not be queried due to a lack of valid authentication credentials (403 Forbidden).
```

I am on a Linux System and use uv 0.4.24. 

Using virtualenv and normal pip works fine, thus I suspect a problem with uv here.

---

_Referenced in [astral-sh/uv#5945](../../astral-sh/uv/issues/5945.md) on 2024-10-18 18:05_

---

_Comment by @charliermarsh on 2024-10-18 18:11_

You probably need `torchaudio==2.0.0+cu118` and the same for `torchvision` and `torch`.

---

_Comment by @charliermarsh on 2024-10-18 18:11_

The hint at the end looks like a red herring. I guess the PyTorch index is (for some reason) returning a 403 for packages that aren't found, which seems wrong to me, but that's a PyTorch bug:

```
❯ curl https://download.pytorch.org/whl/cu118/wheel
<?xml version="1.0" encoding="UTF-8"?>
<Error><Code>AccessDenied</Code><Message>Access Denied</Message><RequestId>2KHYSPNC38BWED53</RequestId><HostId>ZeNiFBKyzFyAMNNKZisld1Oxq9ZnHyJ9oOroNvEfZKp6iibw+ma7UpFTmg5Fs5EFZ//XafwMZ3k=</HostId></Error>%
```

---

_Comment by @Leon0402 on 2024-10-18 18:29_

> You probably need `torchaudio==2.0.0+cu118` and the same for `torchvision` and `torch`.

That seems to work. And it is necessary because of the pytorch bug? Because in theory pip should be able to find that version itself automatically, right?
Can uv deal with the incorrectly returned 403 anyway?

---

_Comment by @Leon0402 on 2024-10-18 19:19_

@charliermarsh  My bad, actually doesn't work. 
```
uv pip install torch==2.0.0+cu118 torchvision==0.15.0+cu118 torchaudio==2.0.0+cu118 --index-url https://download.pytorch.org/whl/cu118
```
Same error as before.

---

_Comment by @FishAlchemist on 2024-10-18 19:44_

> @charliermarsh My bad, actually doesn't work.
> 
> ```
> uv pip install torch==2.0.0+cu118 torchvision==0.15.0+cu118 torchaudio==2.0.0+cu118 --index-url https://download.pytorch.org/whl/cu118
> ```
> 
> Same error as before.

@Leon0402  Use ``--extra-index-url`` instead of ``--index-url``
```bash
uv pip install torch==2.0.0+cu118 torchvision==0.15.0+cu118 torchaudio==2.0.0+cu118 --extra-index-url https://download.pytorch.org/whl/cu118
```
There are actually differences between uv and pip in terms of index resolution. I think the problem here. It's not a bug, but they're just not exactly the same.
https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes

Edit:
It doesn't seem to work, it's not very stable.

---

_Comment by @Leon0402 on 2024-10-21 09:29_

@FishAlchemist It seems to generally work quite well when manually editing the toml file. Here is a version that works for me for instance:

```toml
[project]
name = "name"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "torch==2.4.0+cu118",
    "torchvision==0.19.0+cu118",
]

[tool.uv.sources]
torch = { index = "pytorch" }
torchvision = { index = "pytorch" }

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
```

Related to https://github.com/astral-sh/uv/issues/8341 I think. Probably the CLI tools need a little bit more work and then UV could have some special documentation for torch. I know that torch always is a great source of confusion, because getting it right with cuda is not easy (and probably also not possible right now in the most generic way). 
I have not yet tested how well this approach works in transitive setups though (where a dependency specifies torch, but we need to overwrite it with a specific cuda version) or in a more generic way (where different people need different cuda versions).




---

_Comment by @FishAlchemist on 2024-10-21 09:40_

@Leon0402 There is a PR (https://github.com/astral-sh/uv/pull/6523) document about installing PyTorch for UV. 
Perhaps you could also participate in the discussion and share your insights. Considering the complexities of PyTorch, additional perspectives would be beneficial for refining the documentation.

---

_Comment by @Leon0402 on 2024-10-21 09:54_

@FishAlchemist Love it! I'll have a look later. 

---

_Referenced in [astral-sh/uv#6523](../../astral-sh/uv/pulls/6523.md) on 2024-10-22 07:21_

---

_Comment by @smortezah on 2024-11-19 11:50_

None of those worked for me.

Solution:
```
import os
os.environ["OMP_NUM_THREADS"] = "1"
```

---

_Comment by @charliermarsh on 2024-12-27 14:07_

These should now be fixed in the latest releases.

---

_Closed by @charliermarsh on 2024-12-27 14:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-27 14:07_

---

_Label `bug` added by @charliermarsh on 2024-12-27 14:07_

---

_Comment by @MalteEbner on 2025-01-29 18:16_

I am still encountering this issue.

Using `uv 0.5.25` and `Python 3.10.13` on an amd64 ubuntu machine, calling 

`uv pip install "torch==1.12.0+cu113" "torchvision==0.13.0+cu113" "pytorch-lightning==1.9.5" --extra-index-url https://download.pytorch.org/whl/`

fails with

```
× No solution found when resolving dependencies:
  ╰─▶ Because torch==1.12.0+cu113 has no wheels with a matching Python ABI
      tag (e.g., `cp311`) and you require torch==1.12.0+cu113, we can conclude
      that your requirements are unsatisfiable.

      hint: `torch` was found on https://download.pytorch.org/whl/, but not
      at the requested version (torch==1.12.0+cu113). A compatible version
      may be available on a subsequent index (e.g., https://pypi.org/simple).
      By default, uv will only consider versions that are published on the
      first index that contains a given package, to avoid dependency confusion
      attacks. If all indexes are equally trusted, use `--index-strategy
      unsafe-best-match` to consider all versions from all indexes, regardless
      of the order in which they were defined.

      hint: You require CPython 3.11 (`cp311`), but we only found wheels for
      `torch` (v1.12.0+cu113) with the following Python ABI tags: `cp37m`,
      `cp38`, `cp39`, `cp310`
```

Note that both hints are confusing and don't help debugging the problem.
The second one is even misleading, as the command is run on python `Python 3.10.13`

Using exactly the same command, but without the `uv` at the beginning succeeds.

Using `uv 0.2.25` encounters the same error, but only prints the first part of the error message, not the 2 additional sections.

Up until 3 weeks ago, the uv commands were successful. Since then, nothing in our code change. Thus the reason for the error occurring is probably a change in the way pytorch provides its wheel files, uncovering a differing behaviour between pip and uv.

---

_Comment by @charliermarsh on 2025-01-29 18:23_

If you're seeing those hints, then uv is definitely using Python 3.11. How are you determining that it's using Python 3.10?

---

_Comment by @zanieb on 2025-01-29 18:46_

> The second one is even misleading, as the command is run on python Python 3.10.13

What do you mean by this?

Can you share verbose logs?

---

_Comment by @MalteEbner on 2025-01-30 07:47_

> If you're seeing those hints, then uv is definitely using Python 3.11. How are you determining that it's using Python 3.10?

I am running `python --version` directly before the uv command. 

I am running it in GitHub CI inside a freshly created venv created with uv that is then populated with some packages. This steps then overwrites some of the packages to other versions. The full commands of this step are:

```
uv pip install "numpy<1.24"
uv --version
python --version
uv pip install "torch==1.12.0+cu113" "torchvision==0.13.0+cu113" "pytorch-lightning==1.9.5" --extra-index-url https://download.pytorch.org/whl/cu113
```

The output is

```
  shell: /usr/bin/bash -e {0}
  env:
    GIT_REF: master
    pythonLocation: /datasets/actions-runner/core_gpu_runner_05/hostedtoolcache/Python/3.10.13/x64
    PKG_CONFIG_PATH: /datasets/actions-runner/core_gpu_runner_05/hostedtoolcache/Python/3.10.13/x64/lib/pkgconfig
    Python_ROOT_DIR: /datasets/actions-runner/core_gpu_runner_05/hostedtoolcache/Python/3.10.13/x64
    Python2_ROOT_DIR: /datasets/actions-runner/core_gpu_runner_05/hostedtoolcache/Python/3.10.13/x64
    Python3_ROOT_DIR: /datasets/actions-runner/core_gpu_runner_05/hostedtoolcache/Python/3.10.13/x64
    LD_LIBRARY_PATH: /datasets/actions-runner/core_gpu_runner_05/hostedtoolcache/Python/3.10.13/x64/lib
    PATH: /datasets/actions-runner/core_gpu_runner_05/_work/lightly-core/lightly-core/.venv/bin:/datasets/actions-runner/core_gpu_runner_05/hostedtoolcache/Python/3.10.13/x64/bin:/datasets/actions-runner/core_gpu_runner_05/hostedtoolcache/Python/3.10.13/x64:/home/lightly/.local/bin:/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:/home/lightly/.cargo/bin:/home/lightly/condor/bin:/home/lightly/condor/sbin:/home/lightly/miniconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
Resolved 1 package in 5ms
Uninstalled 1 package in 13ms
Installed 1 package in 24ms
 - numpy==2.0.1
 + numpy==1.23.5
uv 0.5.25
Python 3.10.13
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==1.12.0+cu113 has no wheels with a matching Python ABI
      tag (e.g., `cp311`) and you require torch==1.12.0+cu113, we can conclude
      that your requirements are unsatisfiable.

      hint: `torch` was found on https://download.pytorch.org/whl/, but not
      at the requested version (torch==1.12.0+cu113). A compatible version
      may be available on a subsequent index (e.g., https://pypi.org/simple).
      By default, uv will only consider versions that are published on the
      first index that contains a given package, to avoid dependency confusion
      attacks. If all indexes are equally trusted, use `--index-strategy
      unsafe-best-match` to consider all versions from all indexes, regardless
      of the order in which they were defined.

      hint: You require CPython 3.11 (`cp311`), but we only found wheels for
      `torch` (v1.12.0+cu113) with the following Python ABI tags: `cp37m`,
      `cp38`, `cp39`, `cp310`
Error: Process completed with exit code 1.
```

The reported available packages are correct, see the torch wheels at https://download.pytorch.org/whl/torch/

![Image](https://github.com/user-attachments/assets/7eac1bac-85df-4d9d-8a57-cf0072e81ed9)

---

_Comment by @charliermarsh on 2025-01-30 13:55_

Have you tried `uv pip install --python 3.10 ...`? It's not necessarily the case that the `python` that you get from `python --version` is the exact same Python that uv would use.

---

_Comment by @MalteEbner on 2025-01-31 09:25_

> Have you tried `uv pip install --python 3.10 ...`? It's not necessarily the case that the `python` that you get from `python --version` is the exact same Python that uv would use.

I tried `which python && uv pip install --python 3.10 "torch==1.12.0+cu113" "torchvision==0.13.0+cu113" "pytorch-lightning==1.9.5" --extra-index-url https://download.pytorch.org/whl/cu113`
and got

```
/datasets/actions-runner/core_gpu_runner_03/hostedtoolcache/Python/3.10.13/x64/bin/python
error: Failed to inspect Python interpreter from search path at `/usr/bin/python`
  Caused by: Can't use Python at `/usr/bin/python`
  Caused by: Python executable does not support `-I` flag. Please use Python 3.8 or newer.
```


---

_Comment by @zanieb on 2025-01-31 14:44_

Sorry, it seems like something really weird is going on here. Can you share full verbose logs for that last command?

---

_Comment by @zanieb on 2025-01-31 14:52_

And just to clarify, since I asked for the verbose logs before already, that's 

```
uv pip install --verbose --python 3.10 "torch==1.12.0+cu113" "torchvision==0.13.0+cu113" "pytorch-lightning==1.9.5" --extra-index-url https://download.pytorch.org/whl/cu113
```

---

_Comment by @zanieb on 2025-01-31 14:53_

If this is in GitHub Actions, it'd also be really helpful to share the code / a run in a public repository so we can debug what's going on.

---

_Comment by @MalteEbner on 2025-01-31 16:25_

> Sorry, it seems like something really weird is going on here. Can you share full verbose logs for that last command?

Thanks for helping me debug it! The logs were

```
DEBUG uv 0.5.25
DEBUG Searching for Python 3.10 in virtual environments
DEBUG Found `cpython-3.11.11-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/_work/lightly-core/lightly-core/.venv/bin/python3` (virtual environment)
DEBUG Skipping interpreter at `.venv/bin/python3` from virtual environment: does not satisfy request `3.10`
DEBUG Found `cpython-3.10.13-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python3.10` (search path)
DEBUG Ignoring Python interpreter at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python3.10`: system interpreter not explicitly requested
DEBUG Found `cpython-3.10.13-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python3`: system interpreter not explicitly requested
DEBUG Found `cpython-3.10.13-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python`: system interpreter not explicitly requested
DEBUG Found `cpython-3.10.13-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/python` (search path)
DEBUG Ignoring Python interpreter at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/python`: system interpreter not explicitly requested
DEBUG Found `cpython-3.11.11-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/_work/lightly-core/lightly-core/.venv/bin/python3` (search path)
DEBUG Skipping interpreter at `.venv/bin/python3` from search path: does not satisfy request `3.10`
DEBUG Found `cpython-3.11.11-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/_work/lightly-core/lightly-core/.venv/bin/python` (search path)
DEBUG Skipping interpreter at `.venv/bin/python` from search path: does not satisfy request `3.10`
DEBUG Found `cpython-3.10.13-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python3.10` (search path)
DEBUG Ignoring Python interpreter at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python3.10`: system interpreter not explicitly requested
DEBUG Found `cpython-3.10.13-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python3`: system interpreter not explicitly requested
DEBUG Found `cpython-3.10.13-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/bin/python`: system interpreter not explicitly requested
DEBUG Found `cpython-3.10.13-linux-x86_64-gnu` at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/python` (search path)
DEBUG Ignoring Python interpreter at `/datasets/actions-runner/core_gpu_runner_01/hostedtoolcache/Python/3.10.13/x64/python`: system interpreter not explicitly requested
DEBUG Found `cpython-3.10.14-linux-x86_64-gnu` at `/usr/bin/python3.10` (search path)
DEBUG Ignoring Python interpreter at `/usr/bin/python3.10`: system interpreter not explicitly requested
DEBUG Found `cpython-3.8.10-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/usr/bin/python3`: system interpreter not explicitly requested
DEBUG Failed to inspect Python interpreter from search path at `/usr/bin/python` 
DEBUG Skipping bad interpreter at /usr/bin/python from search path: Python executable does not support `-I` flag. Please use Python 3.8 or newer.
DEBUG Found `cpython-3.10.14-linux-x86_64-gnu` at `/bin/python3.10` (search path)
DEBUG Ignoring Python interpreter at `/bin/python3.10`: system interpreter not explicitly requested
DEBUG Found `cpython-3.8.10-linux-x86_64-gnu` at `/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/bin/python3`: system interpreter not explicitly requested
DEBUG Failed to inspect Python interpreter from search path at `/bin/python` 
DEBUG Skipping bad interpreter at /bin/python from search path: Python executable does not support `-I` flag. Please use Python 3.8 or newer.
error: Failed to inspect Python interpreter from search path at `/usr/bin/python`
  Caused by: Can't use Python at `/usr/bin/python`
  Caused by: Python executable does not support `-I` flag. Please use Python 3.8 or newer.
```


This helped find out the error: When creating the `.venv` with uv, it did not take the python version installed by the GitHub action (3.10), but a newer version found on the system (3.11). 
I fixed this by not installing python via GitHub action at all, but by specifying the version when calling `uv venv .venv`. See the update:

![Image](https://github.com/user-attachments/assets/b444b947-f8b5-4376-ada5-0db929548513)


> If this is in GitHub Actions, it'd also be really helpful to share the code / a run in a public repository so we can debug what's going on.

This is a GitHub action on a custom self-hosted GitHub runner that is inside a docker container.

---

_Comment by @zanieb on 2025-01-31 18:21_

Great thank you! I will open an issue to track an improvement here.

---

_Referenced in [astral-sh/uv#11138](../../astral-sh/uv/issues/11138.md) on 2025-01-31 18:23_

---
