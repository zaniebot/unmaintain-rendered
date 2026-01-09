---
number: 7299
title: UV tries to build from source during package resolution
type: issue
state: closed
author: jdumas
labels:
  - question
assignees: []
created_at: 2024-09-11T17:47:09Z
updated_at: 2025-06-26T21:26:43Z
url: https://github.com/astral-sh/uv/issues/7299
synced_at: 2026-01-07T13:12:17-06:00
---

# UV tries to build from source during package resolution

---

_Issue opened by @jdumas on 2024-09-11 17:47_

Hi. I'm trying to install the setup the following project:

```toml
[project]
name = "myproject"
version = "0.1.0"
description = "My project"
requires-python = ">=3.9"
dependencies = [
    "numpy>=1.23",
    "colorama>=0.4.5",
    "cuda-python",
    "torch>=2.4.1,<2.5",
    "torchvision",
    "plyfile",
    "tqdm",
    "opencv-python",
    "imageio",
    "pycolmap",
]

[project.optional-dependencies]
build = ["setuptools"]
compile = [
    "diff-surfel-rasterization@git+https://github.com/hbb1/diff-surfel-rasterization.git@7bdbd51"
]

[tool.uv.pip]
extra-index-url = ["https://download.pytorch.org/whl/cu124"]

[tool.uv]
no-build-isolation-package = ["diff-surfel-rasterization"]
```

I've tried the various approaches documented in the [build isolation](https://docs.astral.sh/uv/concepts/projects/#build-isolation) page:

```
uv venv
uv pip install setuptools
uv sync --extra build --extra build
```

But they all failed to locate `setuptools` during resolve:

```
❯ uv sync --extra build --extra compile
 Updated https://github.com/hbb1/diff-surfel-rasterization.git (7bdbd51)
error: Failed to download and build: `diff-surfel-rasterization @ git+https://github.com/hbb1/diff-surfel-rasterization.git@7bdbd51`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit code: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'setuptools'
---
```

Note that if I manually comment out the `compile` extra in the `pyproject.toml` to install all the dependencies I need, and then later on uncomment it, it then fails with a different error message (which is probably a separate issue):

```
❯ uv sync --extra build --extra compile
 Updated https://github.com/hbb1/diff-surfel-rasterization.git (7bdbd51)
error: Failed to download and build: `diff-surfel-rasterization @ git+https://github.com/hbb1/diff-surfel-rasterization.git@7bdbd51`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit code: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "C:\devel\myproject\.venv\Lib\site-packages\setuptools\build_meta.py", line 373, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "C:\devel\myproject\.venv\Lib\site-packages\setuptools\build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "C:\devel\myproject\.venv\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 22, in <module>
  File "C:\devel\myproject\.venv\Lib\site-packages\torch\utils\cpp_extension.py", line 1076, in CUDAExtension
    library_dirs += library_paths(cuda=True)
                    ^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\devel\myproject\.venv\Lib\site-packages\torch\utils\cpp_extension.py", line 1214, in library_paths
    paths.append(_join_cuda_home(lib_dir))
                 ^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\devel\myproject\.venv\Lib\site-packages\torch\utils\cpp_extension.py", line 2416, in _join_cuda_home
    raise OSError('CUDA_HOME environment variable is not set. '
OSError: CUDA_HOME environment variable is not set. Please set it to your CUDA install root.
---
```

Would appreciate any guidance on the matter. Thanks!

- **Platform**: Windows
- **UV version**: 0.4.9


---

_Referenced in [prefix-dev/pixi#2033](../../prefix-dev/pixi/issues/2033.md) on 2024-09-11 17:48_

---

_Comment by @charliermarsh on 2024-09-11 18:48_

Interesting... On my machine, I get:

```
❯ uv sync --extra build --extra build
 Updated https://github.com/hbb1/diff-surfel-rasterization.git (7bdbd51)
error: Failed to download and build: `diff-surfel-rasterization @ git+https://github.com/hbb1/diff-surfel-rasterization.git@7bdbd51`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/crmarsh/workspace/puffin/bar/.venv/lib/python3.12/site-packages/setuptools/build_meta.py", line 366, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/Users/crmarsh/workspace/puffin/bar/.venv/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/crmarsh/workspace/puffin/bar/.venv/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 13, in <module>
ModuleNotFoundError: No module named 'torch'
```

Which I assume is the expected result?

---

_Comment by @jdumas on 2024-09-11 20:10_

Hmm right, maybe I had the other packages installed in the venv and for me it was only missing setuptools which I've added later. But it's essentially the same problem (other packages should be installed first before trying to build the repo from source).

---

_Comment by @charliermarsh on 2024-09-11 20:42_

You would need to run `uv sync` before `uv sync --extra compile`, because you need `torch` to be installed prior to `uv sync --extra compile`.

---

_Comment by @jdumas on 2024-09-11 21:10_

`uv sync` without `--extra compile` still tries to build `diff-surfel-rasterization` from source at the resolve stage, and would fail due to missing `torch` or `setuptool` package.

---

_Comment by @charliermarsh on 2024-09-13 12:58_

It looks like `diff-surfel-rasterization` doesn't ship with _any_ static metadata. So we have to build the package even just to resolve its dependencies. There's a chicken-and-egg problem -- in that case, you can't use `uv sync` and `uv lock` on their own. You have to pre-build the package with `uv venv && uv pip install torch setuptools && uv pip install diff-surfel-rasterization` (shorthand) so that either (1) `diff-surfel-rasterization` is already in the environment and/or (2) `diff-surfel-rasterization` is in the uv cache, so that it doesn't need to build it to resolve and install.


---

_Label `question` added by @charliermarsh on 2024-09-13 12:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-13 12:58_

---

_Comment by @jdumas on 2024-09-13 14:41_

I see, thanks. Is this because the `diff-surfel-rasterization` doesn't have a `pyproject.toml`, and is using a straight up `setup.py` that you need to run before being able to resolve anything? Is there any way to workaround this? I can't expect to fix all the imperfectly packaged repo around, although maybe that would be the easiest solution in this case.

Finally, even if I'm able to resolve it correctly, I still have the issue of the unset `CUDA_HOME` variable. Let me know if I should create a separate issue for it.

---

_Comment by @charliermarsh on 2024-09-13 14:46_

Yeah, that's correct. We have to run `setup.py` to get the dependencies for the package. If it had a `pyproject.toml` that included a static `dependencies` field under `[project]`, it wouldn't be necessary.

---

_Comment by @jdumas on 2024-09-13 15:13_

Thanks. After adding a simple [pyproject.toml](https://github.com/jdumas/diff-surfel-rasterization/commit/155efa9590ac73ff5535ca84eb08df1b00934236), I am now stuck due to the missing `CUDA_HOME` env variable:

```toml
[project]
name = "myproject"
version = "0.1.0"
description = "My project"
requires-python = ">=3.10"
dependencies = [
    "numpy>=1.23",
    "colorama>=0.4.5",
    "cuda-python",
    "torch>=2.4.1,<2.5",
    "torchvision",
    "plyfile",
    "tqdm",
    "opencv-python",
    "imageio",
    "pycolmap",
    "diff-surfel-rasterization@git+https://github.com/jdumas/diff-surfel-rasterization.git@155efa9",
]

[tool.uv.pip]
extra-index-url = ["https://download.pytorch.org/whl/cu124"]
```

Yields:

```
❯ uv sync
 Updated https://github.com/jdumas/diff-surfel-rasterization.git (155efa9)
Resolved 34 packages in 299ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: diff-surfel-rasterization @ git+https://github.com/jdumas/diff-surfel-rasterization.git@155efa9590ac73ff5535ca84eb08df1b00934236
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit code: 1
--- stdout:

--- stderr:
C:\Users\jedumas\AppData\Local\uv\cache\builds-v0\.tmpbVUHKB\Lib\site-packages\torch\_subclasses\functional_tensor.py:258: UserWarning: Failed to initialize NumPy: No module named 'numpy' (Triggered internally at C:\actions-runner\_work\pytorch\pytorch\builder\windows\pytorch\torch\csrc\utils\tensor_numpy.cpp:84.)
  cpu = _conversion_method_template(device=torch.device("cpu"))
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "C:\Users\jedumas\AppData\Local\uv\cache\builds-v0\.tmpbVUHKB\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\jedumas\AppData\Local\uv\cache\builds-v0\.tmpbVUHKB\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "C:\Users\jedumas\AppData\Local\uv\cache\builds-v0\.tmpbVUHKB\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 22, in <module>
  File "C:\Users\jedumas\AppData\Local\uv\cache\builds-v0\.tmpbVUHKB\Lib\site-packages\torch\utils\cpp_extension.py", line 1076, in CUDAExtension
    library_dirs += library_paths(cuda=True)
                    ^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\jedumas\AppData\Local\uv\cache\builds-v0\.tmpbVUHKB\Lib\site-packages\torch\utils\cpp_extension.py", line 1214, in library_paths
    paths.append(_join_cuda_home(lib_dir))
                 ^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\jedumas\AppData\Local\uv\cache\builds-v0\.tmpbVUHKB\Lib\site-packages\torch\utils\cpp_extension.py", line 2416, in _join_cuda_home
    raise OSError('CUDA_HOME environment variable is not set. '
OSError: CUDA_HOME environment variable is not set. Please set it to your CUDA install root.
---
```

---

_Comment by @charliermarsh on 2024-09-13 15:59_

Dumb question, but is `CUDA_HOME` set?

---

_Comment by @jdumas on 2024-09-13 16:42_

It's not. But who should set it? I've added the `cuda-python` pip package as a dependency to the [pyproject.toml](https://github.com/jdumas/diff-surfel-rasterization/blob/main/pyproject.toml). Is there a way to have UV specify environment variables for building packages?

---

_Comment by @charliermarsh on 2024-09-14 00:35_

I think you're responsible for setting it, outside of uv. I'm not familiar with building that specific package, but it's not uncommon to need to provide some configuration if you're building packages from source.

---

_Comment by @jdumas on 2024-09-14 01:23_

Ok but the problem is that `CUDA_HOME` should point to where `cuda-python` gets installed by `uv`. How can I know that in advance? Building Cuda kernel is kinda ubiquitous in ML Python code. However I have very little experience with it and I'm struggling to find build instructions that work.

EDIT: If I don't try to build the package (i.e. just install the deps in the pyproject.toml), and then run:
```
uv run python -c "import torch; print(torch.cuda.is_available())"
```

It replies `False`. So at this point I'm not quite sure what's wrong with my setup.

EDIT2: After further investigation it looks like I had the name of a table wrong in my `pyproject.toml`. If I replace `[tool.uv.pip]` with `[tool.uv]` in the `pyproject.toml`, it does reply `True`. So Cuda is properly setup via `pip`, but I still don't know who is supposed to set `CUDA_HOME` and how, etc. Very frustrating :(

Conda has some way to define env vars in their .yml. Is there something similar supported by UV or `pyproject.toml`?

---

_Comment by @matejpekar on 2024-10-01 09:32_

I am running into a similar problem when trying to install `detectron2`:

```toml
[project]
name = "test"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["torch>=2.3.1", "detectron2>=0.6"]

[tool.uv]
no-build-isolation-package = ["detectron2"]

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git" }
```

When running `uv lock` with a clean cache (`uv cache clean`) I get this error:
```bash
Using CPython 3.12.6 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
 Updated https://github.com/facebookresearch/detectron2.git (ebe8b45)
error: Failed to download and build: `detectron2 @ git+https://github.com/facebookresearch/detectron2.git`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'setuptools'
---
```
The workaround is to add `detectron2` after installing all other dependencies, however this is very inconvenient when working on multiple devices.

---

_Referenced in [astral-sh/uv#7846](../../astral-sh/uv/pulls/7846.md) on 2024-10-01 15:35_

---

_Comment by @charliermarsh on 2024-10-10 14:28_

I'm going to close this for now because I _think_ the `CUDA_HOME` piece isn't specific to uv.

---

_Closed by @charliermarsh on 2024-10-10 14:28_

---

_Comment by @jdumas on 2024-10-11 02:50_

What's a good platform to discuss this issue then? While the `CUDA_HOME` issue might be specific to PyTorch, I feel that being able to define environment variables, and being able to have some form of introspection (i.e. refer to the active uv environment) is a useful feature to have.

---

_Comment by @kabouzeid on 2024-10-27 06:50_

> I think you're responsible for setting it, outside of uv. I'm not familiar with building that specific package, but it's not uncommon to need to provide some configuration if you're building packages from source.

This is correct. On Linux, you should download CUDA runfiles from [NVIDIA’s official site](https://developer.nvidia.com/cuda-toolkit) (they have downloads for all versions), or use the module system of your compute cluster. For Windows, consider reading a tutorial on best practices for installation. Either way, you need to set up the CUDA environment yourself, not uv.

If you want a quick fix, you can set up a Conda environment just for CUDA and run uv in it. But honestly, I wouldn’t recommend it—because, well, Conda.

---

_Comment by @mstarodub on 2025-06-26 16:28_

> I'm going to close this for now because I _think_ the `CUDA_HOME` piece isn't specific to uv.

I thought CUDA was supposed to "just work" with uv?
If uv downloads it, why should the user set CUDA_HOME to whatever location uv placed it in? This makes no sense.

---

_Comment by @charliermarsh on 2025-06-26 16:31_

I have never had to set `CUDA_HOME`. If you have a specific scenario we can reproduce feel free to open a new issue!

---

_Comment by @mstarodub on 2025-06-26 16:51_

> If you have a specific scenario

very easy:

```
[user@htc-g040 user]$ mkdir test
[user@htc-g040 user]$ cd test
[user@htc-g040 test]$ uv init -p 3.12 .
Initialized project `test` at `/data/prj/user/test`
[user@htc-g040 test]$ uv add torch numpy
Using CPython 3.12.9
Creating virtual environment at: .venv
Resolved 27 packages in 936ms
Prepared 18 packages in 1m 38s
░░░░░░░░░░░░░░░░░░░░ [0/26] Installing wheels...                                                    warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 26 packages in 1m 40s
 + filelock==3.18.0
 + fsspec==2025.5.1
 + jinja2==3.1.6
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.5
 + numpy==2.3.1
 + nvidia-cublas-cu12==12.6.4.1
 + nvidia-cuda-cupti-cu12==12.6.80
 + nvidia-cuda-nvrtc-cu12==12.6.77
 + nvidia-cuda-runtime-cu12==12.6.77
 + nvidia-cudnn-cu12==9.5.1.17
 + nvidia-cufft-cu12==11.3.0.4
 + nvidia-cufile-cu12==1.11.1.6
 + nvidia-curand-cu12==10.3.7.77
 + nvidia-cusolver-cu12==11.7.1.2
 + nvidia-cusparse-cu12==12.5.4.2
 + nvidia-cusparselt-cu12==0.6.3
 + nvidia-nccl-cu12==2.26.2
 + nvidia-nvjitlink-cu12==12.6.85
 + nvidia-nvtx-cu12==12.6.77
 + setuptools==80.9.0
 + sympy==1.14.0
 + torch==2.7.1
 + triton==3.3.1
 + typing-extensions==4.14.0
[user@htc-g040 test]$ uv add vllm
Resolved 77 packages in 419ms
  × Failed to build `vllm==0.2.5`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stderr]
      /home/user/.cache/uv/builds-v0/.tmp3SkAF5/lib/python3.12/site-packages/torch/_subclasses/functional_tensor.py:276:
      UserWarning: Failed to initialize NumPy: No module named 'numpy' (Triggered internally at
      /pytorch/torch/csrc/utils/tensor_numpy.cpp:81.)
        cpu = _conversion_method_template(device=torch.device("cpu"))
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File
      "/home/user/.cache/uv/builds-v0/.tmp3SkAF5/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 331, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File
      "/home/user/.cache/uv/builds-v0/.tmp3SkAF5/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 301, in _get_build_requires
          self.run_setup()
        File
      "/home/user/.cache/uv/builds-v0/.tmp3SkAF5/lib/python3.12/site-packages/setuptools/build_meta.py",
      line 317, in run_setup
          exec(code, locals())
        File "<string>", line 44, in <module>
      RuntimeError: Cannot find CUDA_HOME. CUDA must be available to build the package.

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen`
        flag to skip locking and syncing.
[user@htc-g040 test]$ 
```

edit: i think i had this with flash-attn too.

---

_Comment by @charliermarsh on 2025-06-26 16:56_

Like I said, can you please create a separate issue with the information we'd need to reproduce, like your operating system and architecture? (vLLM v0.2.5 is extremely old by the way -- you probably want to add a lower-bound constraint to ensure you're not building from source.)

---

_Comment by @mstarodub on 2025-06-26 21:26_

>  can you please create a separate issue with the information we'd need to reproduce

actually, your comment helped - no issue on my side anymore. the package resolution resulted in a very old vllm. i added a `numpy<2.3` constraint to make it happy. thanks!

```
uname -a
Linux htc-g040 4.18.0-147.8.1.el8_2.arc.x86_64 #1 SMP Fri Aug 6 10:16:14 BST 2021 x86_64 x86_64 x86_64 GNU/Linux
```

---

_Referenced in [PrimeIntellect-ai/prime-rl#1309](../../PrimeIntellect-ai/prime-rl/pulls/1309.md) on 2025-11-18 05:56_

---
