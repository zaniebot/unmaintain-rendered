---
number: 12331
title: uv should skip parsing non-platform dependencies or sources
type: issue
state: closed
author: pplmx
labels:
  - question
assignees: []
created_at: 2025-03-20T08:14:48Z
updated_at: 2025-09-12T13:51:39Z
url: https://github.com/astral-sh/uv/issues/12331
synced_at: 2026-01-07T13:12:18-06:00
---

# uv should skip parsing non-platform dependencies or sources

---

_Issue opened by @pplmx on 2025-03-20 08:14_

### Summary

I have a `pyproject.toml` where I configure platform-specific dependencies and source settings as follows:

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "megatron-core ; sys_platform == 'linux'",
]

[tool.uv.sources]
megatron-core = { git = "https://github.com/NVIDIA/Megatron-LM", marker = "sys_platform == 'linux'" }
```

On Linux, everything works fine. However, on Windows, running `uv sync` throws the following error:

```log
uv sync
  × Failed to build `megatron-core @ git+https://github.com/NVIDIA/Megatron-LM`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stderr]
      Traceback (most recent call last):
        ...
      FileNotFoundError: [WinError 3] The system cannot find the path specified.

      hint: This usually indicates a problem with the package or the build environment.
```

### Steps to Reproduce

```bash
# run the following commands on Windows
mkdir example
uv init -p 3.12
# copy the above pyproject.toml to this project
uv sync
```

### Expected Behavior

On non-target platforms (e.g., Windows), uv should skip parsing dependencies or sources that have platform-specific markers, thereby avoiding build errors.

### FYI

```txt
uv 0.6.8
Python 3.12.9
```

---

_Label `enhancement` added by @pplmx on 2025-03-20 08:14_

---

_Comment by @charliermarsh on 2025-03-20 13:43_

`uv sync` performs a universal resolution (see the [docs](https://docs.astral.sh/uv/concepts/resolution/#universal-resolution)), so we need to resolve metadata for `megatron-core` even if it won't ultimately be installed.

---

_Label `enhancement` removed by @charliermarsh on 2025-03-20 13:43_

---

_Label `question` added by @charliermarsh on 2025-03-20 13:43_

---

_Comment by @pplmx on 2025-03-21 02:03_

Since `Megatron-LM` does not support Windows, we use an alternative solution on Windows. In this case, how should the `pyproject.toml` be configured to work properly on Windows?

---

_Comment by @charliermarsh on 2025-03-21 04:06_

Normally I would recommend adding a `[[tool.uv.dependency-metadata]]` for `Megatron-LM` so that the resolver has static metadata available and doesn't need to build the project: https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata. That looks difficult for `Megatron-LM` since they use dynamic metadata and it seems to vary quite a bit? Your best bet might be to lock on Linux, then use `--frozen` when installing on Windows.

---

_Comment by @pplmx on 2025-03-21 05:23_

Thank you, @charliermarsh. Here's what I've tried:

1. Generated `uv.lock` on Linux using the provided `pyproject.toml`.
2. Copied `uv.lock` to Windows.
3. Ran `uv sync` on Windows—this step worked as expected.
4. Installed a package (e.g., `numpy`) on Windows with `uv add numpy --frozen` and received no error output.
5. However, when I ran `uv tree` or `uv pip list`, no packages were listed.

Could you please help me understand why the package isn’t showing up?

---

_Comment by @charliermarsh on 2025-03-21 05:32_

Can you explain what you mean by "no packages were listed", and/or provide me with exact steps I can take to reproduce?

---

_Comment by @pplmx on 2025-03-21 05:55_

## Steps to Reproduce

```bash
# On Linux
mkdir example; cd example
uv init -p 3.12
uv add "git+https://github.com/NVIDIA/Megatron-LM; sys_platform == 'linux'"
# show current packages
uv tree -d2

# On Windows
mkdir example; cd example
uv init -p 3.12
# copy the above pyproject.toml and uv.lock generated on Linux to replace here
uv sync
uv tree -d2
uv add numpy --frozen
uv tree -d2 # This will throw error
uv tree -d2 --frozen # output: example v0.1.0
uv pip list # output nothing
```

---

_Comment by @zanieb on 2025-03-21 13:38_

`uv add --frozen` won't update the lockfile with your new dependency (and `uv tree --frozen` similarly will not update the lockfile). You need to add your dependencies up-front on the Linux machine (to workaround this specific problem).

---

_Comment by @pplmx on 2025-03-22 06:43_

Thank you, @zanieb.
I understand the process now. For this case, to use `numpy` on Windows, I need to run `uv add numpy` on Linux first to generate the `uv.lock` file, and then execute `uv sync --frozen` on Windows using that lock file.


---

_Comment by @pplmx on 2025-03-22 06:46_

Personally, [uv should skip parsing non-platform dependencies or sources](https://github.com/astral-sh/uv/issues/12331#top) is a valid requirement :)

---

_Comment by @zanieb on 2025-04-01 17:16_

I don't think we'll do that. The point of uv's lockfile is that it's platform independent and the problem is rather that this package is not following best practices for declaring metadata.

---

_Closed by @zanieb on 2025-04-01 17:16_

---

_Comment by @pplmx on 2025-04-03 01:54_

Thank you for your assistance.
I'm not entirely sure what the best practices are for declaring metadata. Could you please provide some guidance? 
Additionally, how should a Linux-only package declare the correct metadata? 
Once I have this information, I'll be happy to open an issue on Megatron-LM to further discuss the topic. 
Thanks again!

---

_Comment by @zanieb on 2025-04-03 02:11_

The problem is that we need build the package to determine its metadata, but the package can only be built on Linux. Metadata can be declared statically, so the build does not need to happen. Or, they can build wheels for supported platforms, in which the resolved metadata can be stored. 

For background, see 

- https://packaging.python.org/en/latest/specifications/pyproject-toml/#declaring-project-metadata-the-project-table
- https://peps.python.org/pep-0621/

The project is using a `pyproject.toml`, but specifically defines critical core metadata fields as dynamic: https://github.com/NVIDIA/Megatron-LM/blob/cbc89b322c454a2de46edcbd1fc708669aeafd59/pyproject.toml#L11

---

_Comment by @pplmx on 2025-04-03 03:13_

Thank you for the detailed explanation and the reference links. 


---

_Comment by @mfordiani-cosmohc on 2025-09-12 12:32_

@zanieb @charliermarsh 

I'm having a similar problem that imho provides a coherent use case for some kind of platform-specific exclusion of optional dependencies:

1. I have a Python project that should be able to run on dev machines (amd64) and on NVIDIA Jetson (aarch64)
2. Since there is no publicly available `tensorrt` wheel for Tegra / aarch64, I can only use the pre-installed `tensorrt` Python library inside the NVIDIA `l4t-tensorrt` containers
3. So I define `tensorrt` as an **OPTIONAL** dependency group to be used only during development, trying to keep the version as close as possibile to what is found in the NVIDIA container
```toml
[project.optional-dependencies]
tensorrt = [
    "tensorrt-cu12==10.8.0.43",
    "tensorrt-cu12-bindings==10.8.0.43",
    "tensorrt-cu12-libs==10.8.0.43",
]
polygraphy = [
    "polygraphy>=0.49.24",
]
torch = [
    "torch>=2.7.1",
    "torchaudio>=2.7.1",
    "torchvision>=0.22.1",
]
```
4. And I add a `[tool.uv.sources]` block to specify the allowed platforms for `tensorrt` and relevant index
```toml
[tool.uv.sources]
tensorrt = [
    { index='nvidia', marker = " platform_machine == 'amd64' " },
]

[[tool.uv.index]]
name = "nvidia"
url = "https://pypi.nvidia.com/"
explicit = true
```
Now, what prevents uv from understanding that it shouldn't look for a compatible `tensorrt` on an `aarch64` platform when I run `uv sync --extra torch`? I am JUST asking for `torch`, not anything else.

This behavior defeats the main point of using uv for a similar project, as I cannot sync the project with the extras I need, and then use `uv export` to generate a pinned `requirements.txt` to use in CI, which is my usual workflow.

```
RuntimeError: TensorRT does not currently build wheels for Tegra systems

...

help: `tensorrt-cu12` (v10.8.0.43) was included because `aab[tensorrt]` (v2025.1) depends on `tensorrt-cu12==10.8.0.43`
```

Can we revisit this issue at least for optional dependencies?


---

_Comment by @zanieb on 2025-09-12 13:13_

You should generate the lockfile on a platform where this package is available, then you'll be able to use it on other machines.

Alternatively, you can declare static metadata for the package https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata

---

_Comment by @mfordiani-cosmohc on 2025-09-12 13:46_

thanks for the workaround @zanieb 

In case I went with the static metadata approach, is there any way to find what I should put into `requires-dist` for a given package? Should I look into a `uv.lock` built where the package is available? 

---

_Comment by @zanieb on 2025-09-12 13:51_

You could look at a `uv.lock`, or the `METADATA` file in an installation of the package, e.g.:

```
❯ uv init --package example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv add anyio
Using CPython 3.14.0rc2 interpreter at: /Users/zb/.local/bin/python3.14
Creating virtual environment at: .venv
Resolved 4 packages in 82ms
      Built example @ file:///Users/zb/workspace/uv/example
Prepared 1 package in 5ms
Installed 4 packages in 3ms
 + anyio==4.10.0
 + example==0.1.0 (from file:///Users/zb/workspace/uv/example)
 + idna==3.10
 + sniffio==1.3.1
❯ uv sync
Resolved 4 packages in 2ms
Audited 4 packages in 0.51ms
❯ cat .venv/lib/python3.14/site-packages/anyio-4.10.0.dist-info/METADATA
Metadata-Version: 2.4
Name: anyio
Version: 4.10.0
Summary: High-level concurrency and networking framework on top of asyncio or Trio
Author-email: Alex Grönholm <alex.gronholm@nextday.fi>
License-Expression: MIT
Project-URL: Documentation, https://anyio.readthedocs.io/en/latest/
Project-URL: Changelog, https://anyio.readthedocs.io/en/stable/versionhistory.html
Project-URL: Source code, https://github.com/agronholm/anyio
Project-URL: Issue tracker, https://github.com/agronholm/anyio/issues
Classifier: Development Status :: 5 - Production/Stable
Classifier: Intended Audience :: Developers
Classifier: Framework :: AnyIO
Classifier: Typing :: Typed
Classifier: Programming Language :: Python
Classifier: Programming Language :: Python :: 3
Classifier: Programming Language :: Python :: 3.9
Classifier: Programming Language :: Python :: 3.10
Classifier: Programming Language :: Python :: 3.11
Classifier: Programming Language :: Python :: 3.12
Classifier: Programming Language :: Python :: 3.13
Classifier: Programming Language :: Python :: 3.14
Requires-Python: >=3.9
Description-Content-Type: text/x-rst
License-File: LICENSE
Requires-Dist: exceptiongroup>=1.0.2; python_version < "3.11"
Requires-Dist: idna>=2.8
Requires-Dist: sniffio>=1.1
Requires-Dist: typing_extensions>=4.5; python_version < "3.13"
Provides-Extra: trio
Requires-Dist: trio>=0.26.1; extra == "trio"
```

or, the `METADATA` file from the wheel, e.g.:

```
❯ uv build
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
Successfully built dist/example-0.1.0.tar.gz
Successfully built dist/example-0.1.0-py3-none-any.whl
❯ unzip dist/example-0.1.0-py3-none-any.whl
Archive:  dist/example-0.1.0-py3-none-any.whl
   creating: example/
  inflating: example/__init__.py     
   creating: example-0.1.0.dist-info/
  inflating: example-0.1.0.dist-info/WHEEL  
  inflating: example-0.1.0.dist-info/entry_points.txt  
  inflating: example-0.1.0.dist-info/METADATA  
  inflating: example-0.1.0.dist-info/RECORD  
❯ cat example-0.1.0.dist-info/METADATA
Metadata-Version: 2.3
Name: example
Version: 0.1.0
Summary: Add your description here
Author: Zanie Blue
Author-email: Zanie Blue <contact@zanie.dev>
Requires-Dist: anyio>=4.10.0
Requires-Python: >=3.14.0
Description-Content-Type: text/markdown
```

---
