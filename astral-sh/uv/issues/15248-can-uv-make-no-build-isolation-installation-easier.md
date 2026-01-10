---
number: 15248
title: "Can `uv` make `no-build-isolation` installation easier?"
type: issue
state: closed
author: sidmadala
labels:
  - documentation
assignees: []
created_at: 2025-08-13T03:03:23Z
updated_at: 2025-08-18T21:15:37Z
url: https://github.com/astral-sh/uv/issues/15248
synced_at: 2026-01-10T01:25:54Z
---

# Can `uv` make `no-build-isolation` installation easier?

---

_Issue opened by @sidmadala on 2025-08-13 03:03_

### Summary

`pixi` recently pushed an [update](https://github.com/prefix-dev/pixi/pull/4247) that makes installing `--no-build-isolation` packages much easier and a one-liner. I was wondering if there's any chance of `uv` implementing this type of behavior since it seems a lot more natural than what is currently described in the [docs](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation) (i.e. multi-step environment build, listing build dependencies). For example, if I am installing `flash-attn`, I will most likely be listing/pinning the version of `torch` I will be working with at the top of my `pyproject.toml`, and it seems like it would make sense to resolve those packages before installing any `no-build-isolation` dependencies.

This [issue](https://github.com/astral-sh/uv/issues/14940) and this [comment](https://github.com/astral-sh/uv/issues/1715#issuecomment-2891833039) hint that some people in the community think the above behavior is more natural as well. Any thoughts are much appreciated!

### Example

Here is an example of installing `flash-attn` with `no-build-isolation` via `pixi`
```toml
[project]
name = "test-pixi"
version = "0.1.0"
requires-python = ">=3.11,<3.13"

dependencies = [
    "torch==2.6.0",
    "flash-attn",
]

[build-system]
requires = ["uv_build>=0.8.9,<0.9.0"]
build-backend = "uv_build"

[tool.pixi.project]
# Minimal channel usage: NVIDIA for full CUDA toolkit (with nvcc), conda-forge for Python itself.
channels = ["nvidia", "conda-forge"]
platforms = ["linux-64"]

# Tell pixi we are on a CUDA 12.4 machine so it doesn’t “helpfully” resolve CPU-only stuff.
[tool.pixi.system-requirements]
cuda = "12.4"

# Conda-side deps ONLY for interpreter + CUDA toolkit (provides nvcc)
[tool.pixi.dependencies]
python = "3.12.*"
cuda-toolkit = "12.4.*"

# Disable build isolation for CUDA-extension packages so they can import torch at build time
[tool.pixi.pypi-options]
no-build-isolation = ["flash-attn"]  # This gets installed AFTER torch is resolved/installed
```

The command to install would just be:
```sh
pixi install
```

However, I have to use all the workarounds described in the [docs](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation) to get `flash-attn` installed via `uv`. It seems a bit inconvenient/lower QoL even if it is only a few extra lines of `toml` and a few extra commands. It would be nice to just declaritively write the `pyproject.toml` similar to the above `pixi` spec and do `uv sync`.


---

_Label `enhancement` added by @sidmadala on 2025-08-13 03:03_

---

_Comment by @zanieb on 2025-08-13 03:41_

Yes! We've been hard at work on the flash-attn use-case.

Instead of turning off build isolation, we've invested in extending the build dependencies of packages so they can build properly in isolation

- https://github.com/astral-sh/uv/pull/14735

For flash-attn, you need this build dependency to match your runtime version, so we added support for declaring that as well

- https://github.com/astral-sh/uv/pull/15036

And... for flash-attn, you also need to configure build-time environment variables, so we added that too

- https://github.com/astral-sh/uv/pull/15095

We'll be updating our documentation / guides to reflect these changes soon.

---

_Comment by @zanieb on 2025-08-13 03:42_

See https://github.com/astral-sh/uv/issues/6437#issuecomment-3167274955 for a complete example.

---

_Comment by @sidmadala on 2025-08-13 13:24_

Thanks for all this information! I was wondering how I might go about with this installation using the new methods you outlined above. `axolotl` has optional dependencies `flash-attn`, `deepspeed`, and `vllm`, but is installed with `pip` via `pip install --no-build-isolation axolotl[flash-attn,deepspeed,vllm]`. However, I don't think this behavior works in `uv` right now, or wouldn't be possible given the outline above. Would I need to separate out all these packages and have `no-build-isolation` on all 4 packages simultaneously?

```toml
[project]
name = "test-uv"
version = "0.1.0"
requires-python = ">=3.11,<3.13"

dependencies = [
    "torch==2.6.0",
    "axolotl[deepspeed,flash-attn,vllm]",
]

[build-system]
requires = ["uv_build>=0.8.9,<0.9.0"]
build-backend = "uv_build"

[tool.uv]
no-build-isolation-package = ["axolotl", "flash-attn", "deepspeed", "vllm"]

[tool.uv.sources]
verl = { path = "verl", editable = true }
```

---

_Comment by @charliermarsh on 2025-08-13 13:44_

Hmm, if I understand it correctly, that solution from Pixi requires/assumes that all the build dependencies are also project dependencies? That seems non-ideal to me. (It's not strictly true here, `setuptools` is not a project dependency, but maybe they make it available automatically in the virtual environment.) Our intent is to make it easier to build those packages with build isolation.

I think you want something like this:

```toml
[project]
name = "test-uv"
version = "0.1.0"
requires-python = ">=3.11,<3.13"

dependencies = [
    "torch==2.6.0",
    "axolotl[deepspeed,flash-attn,vllm]",
]

[build-system]
requires = ["uv_build>=0.8.9,<0.9.0"]
build-backend = "uv_build"

[tool.uv.sources]
verl = { path = "verl", editable = true }

[tool.uv.extra-build-dependencies]
flash-attn = [{ requirement = "torch", match-runtime = true }]
deepspeed = [{ requirement = "torch", match-runtime = true }]
axolotl = [{ requirement = "torch", match-runtime = true }]
```

(Looking at the `axolotl` `setup.py` though, I think this will only be right by coincidence, because `axolotl` relies on detecting the `torch` version to inform its declared dependencies, and we can't know the torch version prior to resolving the dependency graph. It looks like they default to 2.6.0, which helps here. (What Pixi is doing in the linked PR would also suffer from this problem.))


---

_Comment by @charliermarsh on 2025-08-13 13:52_

I just tested that on a Linux container and `uv sync` succeeded.

---

_Comment by @sidmadala on 2025-08-13 14:01_

Yep that's a bit more ideal, just have to be careful about knowing what the extra build dependency is. In the example above, does just adding `[tool.uv.extra-build-dependencies]` turn on `no-build-isolation` for those packages, or does it just make sure that the isolated build environment pulls the same version of torch or whatever requirement is needed. Also can we pass in a list to `requirement` if needed? Thanks for the assistance.

---

_Comment by @charliermarsh on 2025-08-13 14:07_

> ...or does it just make sure that the isolated build environment pulls the same version of torch or whatever requirement is needed.

Yeah this is right -- it still does an isolated build, it just lets you augment the dependencies that are included in the build environment (and make sure they match the locked version).

You can pass more requirements like:

```toml
flash-attn = [{ requirement = "torch", match-runtime = true }, "vllm", "setuptools", ...]
```

---

_Comment by @sidmadala on 2025-08-13 16:14_

```sh
(test-uv) sidmadala@icgpu01:~/hocklab/test-uv$ uv sync
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
⠧ axolotl==0.7.0                                                                                 warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusolver-cu12` (v11.7.3.90) or `nvidia-cublas-cu12` (v12.8.4.1).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufile-cu12` (v1.13.1.3) or `nvidia-cublas-cu12` (v12.8.4.1).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufile-cu12` (v1.13.1.3) or `nvidia-cuda-cupti-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufft-cu12` (v11.3.3.83) or `nvidia-cuda-cupti-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufft-cu12` (v11.3.3.83) or `nvidia-cuda-nvrtc-cu12` (v12.8.93).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu12` (v12.8.90) or `nvidia-cuda-nvrtc-cu12` (v12.8.93).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu12` (v12.8.90) or `nvidia-cusparse-cu12` (v12.5.8.93).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusparse-cu12` (v12.5.8.93) or `nvidia-cuda-runtime-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvjitlink-cu12` (v12.8.93) or `nvidia-cuda-runtime-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvjitlink-cu12` (v12.8.93) or `nvidia-curand-cu12` (v10.3.9.90).
⠴ axolotl==0.6.0                                                                                 warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufile-cu12` (v1.13.1.3) or `nvidia-cuda-runtime-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufft-cu12` (v11.3.3.83) or `nvidia-cuda-runtime-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufft-cu12` (v11.3.3.83) or `nvidia-cuda-cupti-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusolver-cu12` (v11.7.3.90) or `nvidia-cuda-cupti-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusolver-cu12` (v11.7.3.90) or `nvidia-cuda-nvrtc-cu12` (v12.8.93).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusparse-cu12` (v12.5.8.93) or `nvidia-cuda-nvrtc-cu12` (v12.8.93).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusparse-cu12` (v12.5.8.93) or `nvidia-curand-cu12` (v10.3.9.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu12` (v12.8.90) or `nvidia-curand-cu12` (v10.3.9.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu12` (v12.8.90) or `nvidia-cublas-cu12` (v12.8.4.1).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvjitlink-cu12` (v12.8.93) or `nvidia-cublas-cu12` (v12.8.4.1).
⠸ axolotl==0.5.2                                                                                 warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufile-cu12` (v1.13.1.3) or `nvidia-cuda-cupti-cu12` (v12.8.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cuda-cupti-cu12` (v12.8.90) or `nvidia-cublas-cu12` (v12.8.4.1).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cusparse-cu12` (v12.5.8.93) or `nvidia-cublas-cu12` (v12.8.4.1).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvjitlink-cu12` (v12.8.93) or `nvidia-cusparse-cu12` (v12.5.8.93).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvjitlink-cu12` (v12.8.93) or `nvidia-cusolver-cu12` (v11.7.3.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu12` (v12.8.90) or `nvidia-cusolver-cu12` (v11.7.3.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu12` (v12.8.90) or `nvidia-curand-cu12` (v10.3.9.90).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-curand-cu12` (v10.3.9.90) or `nvidia-cuda-nvrtc-cu12` (v12.8.93).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufft-cu12` (v11.3.3.83) or `nvidia-cuda-nvrtc-cu12` (v12.8.93).
warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-cufft-cu12` (v11.3.3.83) or `nvidia-cuda-runtime-cu12` (v12.8.90).
  × No solution found when resolving dependencies for split (markers: python_full_version >=
  │ '3.12'):
  ╰─▶ Because only the following versions of axolotl[deepspeed] are available:
          axolotl[deepspeed]==0.5.0
          axolotl[deepspeed]==0.5.1
          axolotl[deepspeed]==0.5.1.post1
          axolotl[deepspeed]==0.5.2
          axolotl[deepspeed]==0.6.0
          axolotl[deepspeed]==0.7.0
          axolotl[deepspeed]==0.8.0
          axolotl[deepspeed]==0.8.1
          axolotl[deepspeed]==0.9.0
          axolotl[deepspeed]==0.9.1
          axolotl[deepspeed]==0.9.1.post1
          axolotl[deepspeed]==0.9.2
          axolotl[deepspeed]==0.10.0
          axolotl[deepspeed]==0.11.0
          axolotl[deepspeed]==0.12.0
          axolotl[deepspeed]==0.12.1
      and axolotl==0.5.0 depends on torch==2.3.1, we can conclude that axolotl[deepspeed]<0.5.1
      depends on torch==2.3.1.
      And because axolotl[deepspeed]>=0.5.1,<=0.5.1.post1 was yanked and axolotl>=0.5.2 depends
      on torch==2.8.0, we can conclude that axolotl[deepspeed]<0.6.0 depends on one of:
          torch==2.3.1
          torch>=2.8.0

      And because axolotl>=0.6.0 depends on torch==2.8.0 and torch==2.8.0, we can conclude that
      axolotl[deepspeed]<0.8.0 depends on one of:
          torch==2.3.1
          torch>=2.8.0

      And because axolotl>=0.8.0 depends on torch==2.8.0 and torch==2.8.0, we can conclude that
      axolotl[deepspeed]<0.9.0 depends on one of:
          torch==2.3.1
          torch>=2.8.0

      And because axolotl>=0.9.0 depends on torch==2.8.0 and torch==2.8.0, we can conclude that
      axolotl[deepspeed]<0.9.1.post1 depends on one of:
          torch==2.3.1
          torch>=2.8.0

      And because axolotl>=0.9.1.post1 depends on torch==2.8.0 and torch==2.8.0, we can
      conclude that all of:
          axolotl[deepspeed]<0.9.2
          axolotl[deepspeed]>0.9.2,<0.11.0
      depend on one of:
          torch==2.3.1
          torch>=2.8.0

      And because all of:
          axolotl==0.9.2
          axolotl>=0.11.0
      depend on torch==2.8.0 and torch==2.8.0, we can conclude that all of:
          axolotl[deepspeed]<0.9.2
          axolotl[deepspeed]>0.9.2,<0.12.1
      depend on one of:
          torch==2.3.1
          torch>=2.8.0

      And because all of:
          axolotl==0.9.2
          axolotl>=0.12.1
      depend on torch==2.8.0 and torch==2.8.0, we can conclude that all versions of
      axolotl[deepspeed] depend on one of:
          torch==2.3.1
          torch>=2.8.0

      And because your project depends on axolotl[deepspeed] and torch==2.6.0, we can conclude
      that your project's requirements are unsatisfiable.

      hint: Pre-releases are available for `axolotl` in the requested range (e.g., 0.8.0.dev0),
      but pre-releases weren't enabled (try: `--prerelease=allow`)

      hint: While the active Python version is 3.11, the resolution failed for other Python
      versions supported by your project. Consider limiting your project's supported Python
      versions using `requires-python`.
```

I end up getting all these warnings and eventually `uv sync` doesn't work. My `nvcc --version==11.5.119` which should be high enough to at least get [`flash-attn=2.7.3`](https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu11torch2.6cxx11abiTRUE-cp312-cp312-linux_x86_64.whl) and shouldn't really affect the `axolotl` download.

---

_Comment by @charliermarsh on 2025-08-13 16:18_

Did you try narrowing your supported Python version range, e.g., to 3.12?

---

_Comment by @sidmadala on 2025-08-13 16:19_

Yup I tried narrowing the range to 3.12 and adding `--prerelease==allow` to no avail.

---

_Comment by @charliermarsh on 2025-08-13 16:20_

Sorry, can you share the exact `pyproject.toml`? The one I shared above resolved for me without issue.

---

_Comment by @sidmadala on 2025-08-13 16:21_

```toml
[project]
name = "test-uv"
version = "0.1.0"
requires-python = "==3.12.*"

dependencies = ["torch==2.6.0", "axolotl[deepspeed,flash-attn,vllm]"]

[build-system]
requires = ["uv_build>=0.8.9,<0.9.0"]
build-backend = "uv_build"

[tool.uv.sources]
verl = { path = "verl", editable = true }

[tool.uv.extra-build-dependencies]
flash-attn = [{ requirement = "torch", match-runtime = true }]
deepspeed = [{ requirement = "torch", match-runtime = true }]
axolotl = [{ requirement = "torch", match-runtime = true }]
```

Here is what I'm trying currently.

---

_Comment by @charliermarsh on 2025-08-13 16:50_

I think you actually need to _remove_ `axolotl = [{ requirement = "torch", match-runtime = true }]` (related to the issue I mentioned above). I'll file a separate issue for thinking about how to make that clearer.

---

_Comment by @sidmadala on 2025-08-13 17:10_

Here's the new `pyproject.toml`

```toml
[project]
name = "test-uv"
version = "0.1.0"
requires-python = "==3.12.*"

dependencies = ["torch==2.6.0", "axolotl[deepspeed,flash-attn,vllm]"]

[build-system]
requires = ["uv_build>=0.8.9,<0.9.0"]
build-backend = "uv_build"

[tool.uv.sources]
verl = { path = "verl", editable = true }

[tool.uv.extra-build-dependencies]
flash-attn = [{ requirement = "torch", match-runtime = true }]
# deepspeed = [{ requirement = "torch", match-runtime = true }]
# axolotl = [{ requirement = "torch", match-runtime = true }]
```

and the new error even though `axolotl==0.12.1` clearly has a `vllm` extension in the `setup.py`.

```sh
sidmadala@icgpu01:~/hocklab/test-uv$ uv sync
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
warning: Resolving despite existing lockfile due to fork markers not covering the supported environments: `python_version < '0'` vs `python_full_version == '3.12.*'`
Resolved 235 packages in 39.16s
warning: The package `axolotl==0.12.1` does not have an extra named `vllm`
```

I also noticed that `flash-attn>=0.8.0` was trying to be built which doesn't match the system requirements. Do I just have to pin which version I want and it'll figure out the rest?


As for documentation, I think it would be good to outline how packages with extras that require `no-build-isolation` might be easily installed. Perhaps `axolotl` itself would be a good package to demonstrate this considering there is a mix of both `no-build-isolation` packages (i.e. `flash-attn`) and packages which are fine in isolation (i.e. `deepspeed`, `vllm`).

---

_Comment by @charliermarsh on 2025-08-13 17:11_

It looks like axolotl removes the `vllm` extra if you're using PyTorch 2.6.0: https://github.com/axolotl-ai-cloud/axolotl/blob/e0a2523a3bf875c9e82de0ecea67371e634553f1/setup.py#L81

---

_Comment by @sidmadala on 2025-08-13 19:02_

Yea it looks like there's some brittleness in `axolotl`. Closing the issue might be the right move since it works on your environment.

---

_Comment by @charliermarsh on 2025-08-14 10:48_

(I believe each vLLM version is pinned to a specific PyTorch patch version. It's possible that they never published a vLLM that's compatible with PyTorch 2.6.0? But I didn't check that myself -- just speculating.)

---

_Comment by @charliermarsh on 2025-08-14 10:48_

I'll keep this open for now and close it when I improve the documentation around these concepts. I want to include examples for FlashAttention, DeepSpeed, and Axolotl.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-14 10:48_

---

_Label `enhancement` removed by @charliermarsh on 2025-08-14 10:49_

---

_Label `documentation` added by @charliermarsh on 2025-08-14 10:49_

---

_Comment by @sidmadala on 2025-08-14 15:03_

> (I believe each vLLM version is pinned to a specific PyTorch patch version. It's possible that they never published a vLLM that's compatible with PyTorch 2.6.0? But I didn't check that myself -- just speculating.)

I think there are versions that are compatible. I took Axolotl out of my requirements and was able to resolve the following pyproject.toml in one go. In reality I think it's because there were ABI issues with certain versions of vLLM and Axolotl is pretty strict on their dependency ranges.


```toml
[project]
name = "test-uv"
version = "0.1.0"
requires-python = ">=3.11,<3.13"

dependencies = [
    "torch==2.6.0",
    "torchvision==0.21.0",
    "torchaudio==2.6.0",
    "flash-attn==2.7.4.post1", # flash-attn>=2.8.0 has ABI issues with torch 2.6.0
    "vllm>=0.8.1",
    "deepspeed>=0.17.0",
    # "axolotl[deepspeed,flash-attn,vllm]",
    "huggingface-hub[cli,torch]>=0.34.4",
    "transformers",
    "evaluate",
    "datasets",
    "accelerate",
    "peft",
    "bitsandbytes",
    "trl",
    "unsloth",
    "sentencepiece",
    "openai",
    "scikit-learn",
    "scipy",
    "jupyterlab",
    "matplotlib",
    "seaborn",
    "gradio",
    "wandb",
    "nltk",
    "pandas",
    "importlib_resources",
    "importlib_metadata",
    "typeguard",
    "python-dotenv",
    "nvitop",
    "gpustat",
    "ruff",
    "mypy",
]

# [build-system]
# requires = ["uv_build>=0.8.9,<0.9.0"]
# build-backend = "uv_build"

[tool.uv]
# no-binary-package = ["flash-attn"]
# no-build-isolation-package = ["flash-attn"]
# no-build-isolation-package = ["flash-attn", "deepspeed"]
# no-build-isolation-package = ["flash-attn", "deepspeed", "axolotl"]

[tool.uv.sources]
verl = { path = "verl", editable = true }

[tool.uv.extra-build-dependencies]
flash-attn = [{ requirement = "torch", match-runtime = true }]
deepspeed = [{ requirement = "torch", match-runtime = true }]
# axolotl = [{ requirement = "torch", match-runtime = true }]
```

---

_Referenced in [astral-sh/uv#15301](../../astral-sh/uv/issues/15301.md) on 2025-08-15 10:18_

---

_Referenced in [astral-sh/uv#15306](../../astral-sh/uv/pulls/15306.md) on 2025-08-15 13:38_

---

_Referenced in [astral-sh/uv#15326](../../astral-sh/uv/pulls/15326.md) on 2025-08-16 15:51_

---

_Closed by @zanieb on 2025-08-18 21:15_

---
