```yaml
number: 9654
title: "`uv sync --no-group` incorrectly installs from the excluded group if `tensorrt-cu12-libs==10.3.0` package is a dependency, but correctly excludes for other packages"
type: issue
state: closed
author: akiyamasho
labels:
  - question
assignees: []
created_at: 2024-12-05T04:57:51Z
updated_at: 2025-02-09T19:26:43Z
url: https://github.com/astral-sh/uv/issues/9654
synced_at: 2026-01-10T03:50:30Z
```

# `uv sync --no-group` incorrectly installs from the excluded group if `tensorrt-cu12-libs==10.3.0` package is a dependency, but correctly excludes for other packages

---

_Issue opened by @akiyamasho on 2024-12-05 04:57_

### Overview

Hello! I was wondering how we can actually exclude groups in `dependency-group`, because running `uv sync --no-group` still incorrectly installs from the excluded group.

### Replication Steps

Based on `uv sync --help`, `--no-group` should exclude dependencies from the specified group:

```
Usage: uv sync [OPTIONS]

Options:
      ...
      **--no-group <NO_GROUP>
          Exclude dependencies from the specified dependency group**
```

However, it does not exclude it at all.

As an example, I created a simple project generated from `uv init` here:
https://github.com/akiyamasho/astral-uv-no-group-bug

The `pyproject.toml` is as follows:
```toml
[project]
name = "astral-uv-no-group-bug"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "pillow==11.0.0",
]

# Even adding `; sys_platform != 'darwin'` and running on MacOS does not work
[dependency-groups]
gpu = ["tensorrt-cu12-libs==10.3.0"]
```

When running `uv sync --no-group gpu`, it still installs the TensorRT dependency incorrectly.

```
(py3.12) user@computer astral-uv-no-group-bug % uv sync --no-group gpu
Using CPython 3.12.6 interpreter at: python
Creating virtual environment at: .venv
Building tensorrt-cu12-libs==10.3.0
⠸ tensorrt-cu12-libs==10.3.0                                                                    × Failed to download and build `tensorrt-cu12-libs==10.3.0`
  ╰─▶ Build backend failed to build wheel through `build_wheel` (exit status: 1)

      [stderr]
      INFO:nvidia-stub:Testing wheel
      tensorrt_cu12_libs-10.3.0-py2.py3-none-manylinux_2_17_x86_64.whl against tag
      py3-none-manylinux_2_17_x86_64
      INFO:nvidia-stub:Testing wheel
      tensorrt_cu12_libs-10.3.0-py2.py3-none-manylinux_2_17_x86_64.whl against tag
      py2-none-manylinux_2_17_x86_64
      INFO:nvidia-stub:Testing wheel tensorrt_cu12_libs-10.3.0-py2.py3-none-win_amd64.whl
      against tag py2-none-win_amd64
      INFO:nvidia-stub:Testing wheel tensorrt_cu12_libs-10.3.0-py2.py3-none-win_amd64.whl
      against tag py3-none-win_amd64
        File
      "~/Library/Caches/uv/builds-v0/.tmpByxLJR/lib/python3.12/site-packages/nvidia_stub/wheel.py",
      line 177, in download_wheel
          return download_manual(wheel_directory, distribution, version)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File
      "~/Library/Caches/uv/builds-v0/.tmpByxLJR/lib/python3.12/site-packages/nvidia_stub/wheel.py",
      line 144, in download_manual
          raise RuntimeError(f"Didn't find wheel for {distribution} {version}")
      Traceback (most recent call last):
        File
      "~/Library/Caches/uv/builds-v0/.tmpByxLJR/lib/python3.12/site-packages/nvidia_stub/wheel.py",
      line 177, in download_wheel
          return download_manual(wheel_directory, distribution, version)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File
      "~/Library/Caches/uv/builds-v0/.tmpByxLJR/lib/python3.12/site-packages/nvidia_stub/wheel.py",
      line 144, in download_manual
          raise RuntimeError(f"Didn't find wheel for {distribution} {version}")
      RuntimeError: Didn't find wheel for tensorrt-cu12-libs 10.3.0

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File
      "~/Library/Caches/uv/builds-v0/.tmpByxLJR/lib/python3.12/site-packages/nvidia_stub/buildapi.py",
      line 29, in build_wheel
          return download_wheel(pathlib.Path(wheel_directory), config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File
      "~/Library/Caches/uv/builds-v0/.tmpByxLJR/lib/python3.12/site-packages/nvidia_stub/wheel.py",
      line 179, in download_wheel
          report_install_failure(distribution, version, exception_context)
        File
      "~/Library/Caches/uv/builds-v0/.tmpByxLJR/lib/python3.12/site-packages/nvidia_stub/error.py",
      line 63, in report_install_failure
          raise InstallFailedError(
      nvidia_stub.error.InstallFailedError:
      *******************************************************************************

      The installation of tensorrt-cu12-libs for version 10.3.0 failed.

      This is a special placeholder package which downloads a real wheel package
      from https://pypi.nvidia.com. If https://pypi.nvidia.com is not reachable, we
      cannot download the real wheel file to install.

      You might try installing this package via
      ```
      $ pip install --extra-index-url https://pypi.nvidia.com tensorrt-cu12-libs
      ```

      Here is some debug information about your platform to include in any bug
      report:

      Python Version: CPython 3.12.4
      Operating System: Darwin 23.6.0
      CPU Architecture: arm64
      nvidia-smi command not found. Ensure NVIDIA drivers are installed.

      *******************************************************************************
```

### Final Notes

My main goal is to be able to easily install and develop on CPU locally and setup CI with GPU libraries for deployment.
Is the documentation incorrect or was there something incorrect in the way the groups are defined?

---

_Comment by @akiyamasho on 2024-12-05 05:34_

Hello again! There's this very strange behavior I've noticed.

So I tried using a different library for exclusion to test it out, and what's interesting is `tensorrt-cu12==10.3.0` or any other library is excluded correctly with `uv sync --no-group gpu` and even when using `; sys_platform != 'darwin'` directly in `pyproject.toml`:

### ✅ This is Excluded Correctly

```toml
[project]
name = "astral-uv-no-group-bug"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "pillow==11.0.0",
]

[dependency-groups]
gpu = ["tensorrt-cu12==10.3.0"]
```

but if I use `tensorrt-cu12-libs==10.3.0`, it still tries to install it regardless of `uv sync --no-group gpu` or using `; sys_platform != 'darwin'` directly in `pyproject.toml`:

### ❌ This is NOT Excluded Correctly:

```toml
[project]
name = "astral-uv-no-group-bug"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "pillow==11.0.0",
]

# Even adding `; sys_platform != 'darwin'` and running on MacOS does not work
[dependency-groups]
gpu = ["tensorrt-cu12-libs==10.3.0"]
```

Is this an issue with UV or some compatibility with the specific `tensorrt-cu12-libs` package? I assume any package should be excluded given the `--no-group` solution or `sys_platform` solution.

---

_Comment by @zanieb on 2024-12-05 14:54_

Does the error occur during `uv lock`? It seems likely the error is during _resolution_ in which we need to build the package to determine its metadata?

---

_Label `question` added by @charliermarsh on 2024-12-06 00:04_

---

_Comment by @charliermarsh on 2024-12-06 00:09_

Yeah we need to be able to resolve the project, even on macOS.

---

_Renamed from "`uv sync --no-group` still incorrectly installs from the excluded group" to "`uv sync --no-group` incorrectly installs from the excluded group if `tensorrt-cu12-libs==10.3.0` package is a dependency, but correctly excludes for other packages" by @akiyamasho on 2024-12-06 02:13_

---

_Closed by @charliermarsh on 2024-12-26 16:54_

---

_Comment by @validatedev on 2025-02-09 12:56_

@charliermarsh Any update? It is still happening by the way.


---

_Comment by @charliermarsh on 2025-02-09 14:55_

I don’t know what you’re referring to. You’ll need to file a separate issue with details to reproduce whatever you’re experiencing.

---

_Comment by @validatedev on 2025-02-09 16:30_

The same issue has been occurring, which is why I commented here. I didn't see any related PR that addresses the problem, so I thought you mistakenly marked it as completed. Seems the issue has regressed then.

I'll file a separate issue, as I have noticed that it happens with both group and extra.

---

_Comment by @zanieb on 2025-02-09 18:33_

This was closed because the behavior is expected, we need to resolve all the dependencies — there's no evidence of incorrect _installation_ here.

---

_Comment by @zanieb on 2025-02-09 18:34_

See also https://github.com/astral-sh/uv/issues/11104

---

_Comment by @validatedev on 2025-02-09 19:26_

@zanieb Thanks for the comment. I recently opened the issue https://github.com/astral-sh/uv/issues/11363 to elaborate on my point of view. We may need to resolve the dependencies, but the caveat is that uv tries to build the package regardless of the environment it is working in and the markers or conflicts that are set.

---
