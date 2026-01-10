---
number: 9763
title: "Can't run tensorrt_llm with uv"
type: issue
state: open
author: twmht
labels:
  - question
assignees: []
created_at: 2024-12-10T04:02:55Z
updated_at: 2025-08-08T17:34:44Z
url: https://github.com/astral-sh/uv/issues/9763
synced_at: 2026-01-10T01:24:45Z
---

# Can't run tensorrt_llm with uv

---

_Issue opened by @twmht on 2024-12-10 04:02_

Hi,

I use uv (from python3.10.12)  to install [tensort_llm](https://nvidia.github.io/TensorRT-LLM/installation/linux.html)

```bash
uv pip install tensorrt_llm
```

and then import tensorrt_llm

```pythom
import tensorrt_llm
```

but it would show

```
Traceback (most recent call last):
  File "/home/shared/WhisperS2T/examples/test_import.py", line 1, in <module>
    import tensorrt_llm
  File "/home/acer/.venv/tensorrt-llm/lib/python3.10/site-packages/tensorrt_llm/__init__.py", line 32, in <module>
    import tensorrt_llm.functional as functional
  File "/home/acer/.venv/tensorrt-llm/lib/python3.10/site-packages/tensorrt_llm/functional.py", line 25, in <module>
    import tensorrt as trt
  File "/home/acer/.venv/tensorrt-llm/lib/python3.10/site-packages/tensorrt/__init__.py", line 18, in <module>
    from tensorrt_bindings import *
ModuleNotFoundError: No module named 'tensorrt_bindings'
```

But if you install it through pip, there will be no problems at all.

Any idea?




---

_Comment by @rohnsha0 on 2024-12-10 07:04_

using `uv add` doesn't help?

---

_Comment by @twmht on 2024-12-10 07:10_

I didn't try `uv add`, but what is the big difference between it and `uv pip install`?

---

_Comment by @twmht on 2024-12-10 07:14_

Based on my current observations, it seems that using uv pip install tensorrt_llm misses some dependencies during installation.

For example, when I install it using the regular pip install, it automatically installs tensorrt-bindings and tensorrt-libs as well.

```
tensorrt==9.2.0.post12.dev5
tensorrt-bindings==9.2.0.post12.dev5
tensorrt-libs==9.2.0.post12.dev5
tensorrt-llm==0.8.0.dev2024012301
```

However, uv only installs tensorrt and does not install tensorrt-bindings or tensorrt-libs.

---

_Comment by @charliermarsh on 2024-12-10 13:33_

Can you include the `--verbose` logs from `uv pip install`? (Are you sure that you installed from the nvidia index, and that you activated the virtual environment?)

---

_Label `question` added by @charliermarsh on 2024-12-10 13:33_

---

_Comment by @konstin on 2024-12-10 17:04_

Can reproduce in `nvidia/cuda:12.6.3-cudnn-devel-ubuntu24.04`. The pypi packages uses https://github.com/wheel-next/wheel-stub as build backend, maybe @emmatyping-nv knows.

---

_Comment by @emmatyping-nv on 2024-12-10 19:44_

Does uv inspect the PKG-INFO to decide dependencies or does it build the wheel and look at what the backend generates? `tensorrt_cu12` does a similar thing to wheel-stub to acquire the real wheel via an sdist build, but I don't think it lists it's dependencies in the PKG-INFO.

---

_Comment by @charliermarsh on 2024-12-10 19:48_

If available for PyPI dependencies, yes, we'll use PKG-INFO. If not, we ask the backend. It's all heuristics so we could probably refine it.

---

_Comment by @melMass on 2024-12-27 12:21_

I solved this issue by using `--pre`:

```diff
uv add --group trt tensorrt --pre # I'm using a group, omit it for regular dep
# Ignoring existing lockfile due to change in pre-release mode: `if-necessary-or-explicit` vs. `allow`
# Resolved 153 packages in 883ms
#   Built tensorrt==10.7.0
# Prepared 7 packages in 36.67s
# Uninstalled 5 packages in 198ms
# Installed 8 packages in 207ms
- beautifulsoup4==4.12.3
+ beautifulsoup4==4.13.0b2
- cython==3.0.11
+ cython==3.1.0a1
- omegaconf==2.3.0
+ omegaconf==2.4.0.dev3
- safetensors==0.4.5
+ safetensors==0.4.6.dev0
- scipy==1.14.1
+ scipy==1.15.0rc2
+ tensorrt==10.7.0
+ tensorrt-bindings==9.3.0.post12.dev1
+ tensorrt-cu12==10.7.0
```

---

_Comment by @emmatyping-nv on 2024-12-30 16:18_

Perhaps after resolving dependencies via `PKG-INFO`, after the wheel build you could verify the generated wheel dependencies match the `PKG-INFO`, and backtrack if they disagree. This lets the current behavior continue as a pre-fetch optimization while still ending up with the correct result.

---

_Comment by @konstin on 2025-01-02 15:52_

There's a number of packages with `PKG-INFO` that doesn't match the final `METADATA`, so i'm afraid we can't dismiss those packages.

---

_Comment by @emmatyping-nv on 2025-01-02 16:21_

> There's a number of packages with PKG-INFO that doesn't match the final METADATA, so i'm afraid we can't dismiss those packages.

I think if you re-solve dependencies with the new requirements if the PKG-INFO and METADATA do not match, you won't have issues unless information is somehow not captured in the METADATA file that is captured in the PKG-INFO (which should not happen unless a build backend is misbehaving).

To be more precise with what I said above, I'm imagining uv having the following behavior:

1. uv selects an sdist to be installed (say, as part of normal package resolution)
2. you download the dependencies from PKG-INFO, and store the list of those
3. you build the sdist into a wheel (probably concurrently with the downloads in (2)
4. you check if the METADATA matches the PKG-INFO *requirements/extras only*
5. Then you either
    a. find that they match and continue with the resolution normally
    b. find that they differ and feed this information back to the resolver so that it can take this additional information into account

This way if the requirements listed in METADATA matches PKG-INFO, you get the current speed benefits with a tiny bit of extra checks. If they differ, package installation/resolution will be slower, but still eventually come out with the correct answer.

---

_Comment by @emmatyping-nv on 2025-01-02 17:00_

@melMass 
> I solved this issue by using --pre

I would recommend instead to [use an index](https://docs.astral.sh/uv/configuration/indexes/#defining-an-index) to point to the official NVIDIA PyPI server and the sub-packages `tensorrt-cu12` points to:

```toml
[project]
dependencies = [
    "tensorrt-cu12-libs", # ref: https://github.com/astral-sh/uv/issues/9763
    "tensorrt-cu12-bindings", # ref: https://github.com/astral-sh/uv/issues/9763
]

[tool.uv.sources]
tensorrt-cu12-libs = { index = "nvidia" }
tensorrt-cu12-bindings = { index = "nvidia" }

[[tool.uv.index]]
name = "nvidia"
url = "https://pypi.nvidia.com/"
```

---

_Comment by @henryzhuhr on 2025-08-08 17:34_

my `pyproject.toml` is:
```toml
[project]
name = "my project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11, <3.13"
dependencies = [
    "huggingface-hub[cli]>=0.34.3",
    "loguru>=0.7.3",
    "transformers[torch]>=4.54.1",
]
```

When I use the command `uv add tensorrt-llm` to add the dependency, I encounter the following error:
```bash
error: Package `flashinfer` attempted to resolve via URL: git+https://github.com/flashinfer-ai/flashinfer.git@06309c4e. URL dependencies must be expressed as direct requirements or constraints. Consider adding `flashinfer @ git+https://github.com/flashinfer-ai/flashinfer.git@06309c4e` to your dependencies or constraints file.
```

However, when I add with `uv pip install tensorrt-llm`, everything works fine.

---
