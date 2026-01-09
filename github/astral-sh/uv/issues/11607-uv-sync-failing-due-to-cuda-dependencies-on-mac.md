---
number: 11607
title: uv sync failing due to cuda dependencies on MAC when I specifically put them in a cuda group
type: issue
state: closed
author: spkgyk
labels:
  - bug
assignees: []
created_at: 2025-02-18T22:58:09Z
updated_at: 2025-02-19T00:27:37Z
url: https://github.com/astral-sh/uv/issues/11607
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync failing due to cuda dependencies on MAC when I specifically put them in a cuda group

---

_Issue opened by @spkgyk on 2025-02-18 22:58_

### Summary

Here is my config:
```
[project]
name = "inference-server"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.12.9"
dependencies = [
    "cellpose<3.1",
    "colored>=2.3.0",
    "docker>=7.1.0",
    "fastapi>=0.115.8",
    "fire>=0.7.0",
    "google-cloud-storage>=3.0.0",
    "monai>=1.4.0",
    "onnx>=1.17.0",
    "onnxruntime>=1.20.1",
    "pytorch-ignite>=0.5.1",
    "ray[serve]>=2.42.1",
    "segment-anything>=1.0",
    "tensorboard>=2.19.0",
    "torch>=2.6.0",
    "torchaudio>=2.6.0",
    "torchvision>=0.21.0",
    "tritonclient[http, grpc]>=2.54.0",
]

[dependency-groups]
dev = ["pre-commit>=4.1.0", "pytest>=8.3.4", "pytest-cov>=6.0.0", "ruff>=0.9.6"]
cuda = [
    "cuda-python>=12.8.0",
    "onnx-graphsurgeon>=0.5.5",
    "polygraphy>=0.49.14",
    "tensorrt-cu12>=10.8.0.43",
    "tensorrt-cu12-bindings>=10.8.0.43",
    "tensorrt-cu12-libs>=10.8.0.43",
    "tritonclient[all]>=2.54.0"
]
```
Im on a mac m3. When I run uv sync I get the error:

```
 uv sync
  × Failed to build `tensorrt-cu12-libs==10.8.0.43`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `wheel_stub.buildapi.build_wheel` failed (exit status: 1)

      [stderr]
      INFO:wheel-stub:Testing wheel tensorrt_cu12_libs-10.8.0.43-py2.py3-none-manylinux_2_28_x86_64.whl against tag py3-none-manylinux_2_28_x86_64
      INFO:wheel-stub:Testing wheel tensorrt_cu12_libs-10.8.0.43-py2.py3-none-manylinux_2_28_x86_64.whl against tag py2-none-manylinux_2_28_x86_64
      INFO:wheel-stub:Testing wheel tensorrt_cu12_libs-10.8.0.43-py2.py3-none-manylinux_2_31_aarch64.whl against tag py3-none-manylinux_2_31_aarch64
      INFO:wheel-stub:Testing wheel tensorrt_cu12_libs-10.8.0.43-py2.py3-none-manylinux_2_31_aarch64.whl against tag py2-none-manylinux_2_31_aarch64
      INFO:wheel-stub:Testing wheel tensorrt_cu12_libs-10.8.0.43-py2.py3-none-win_amd64.whl against tag py3-none-win_amd64
      INFO:wheel-stub:Testing wheel tensorrt_cu12_libs-10.8.0.43-py2.py3-none-win_amd64.whl against tag py2-none-win_amd64
        File "/Users/cvaxh/.cache/uv/builds-v0/.tmp6mLMjB/lib/python3.12/site-packages/wheel_stub/wheel.py", line 249, in download_wheel
          return download_manual(wheel_directory, distribution, version, config)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/cvaxh/.cache/uv/builds-v0/.tmp6mLMjB/lib/python3.12/site-packages/wheel_stub/wheel.py", line 185, in download_manual
          raise RuntimeError(f"Didn't find wheel for {distribution} {version}")
      Traceback (most recent call last):
        File "/Users/cvaxh/.cache/uv/builds-v0/.tmp6mLMjB/lib/python3.12/site-packages/wheel_stub/wheel.py", line 249, in download_wheel
          return download_manual(wheel_directory, distribution, version, config)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/cvaxh/.cache/uv/builds-v0/.tmp6mLMjB/lib/python3.12/site-packages/wheel_stub/wheel.py", line 185, in download_manual
          raise RuntimeError(f"Didn't find wheel for {distribution} {version}")
      RuntimeError: Didn't find wheel for tensorrt-cu12-libs 10.8.0.43

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/Users/cvaxh/.cache/uv/builds-v0/.tmp6mLMjB/lib/python3.12/site-packages/wheel_stub/buildapi.py", line 29, in build_wheel
          return download_wheel(pathlib.Path(wheel_directory), config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/cvaxh/.cache/uv/builds-v0/.tmp6mLMjB/lib/python3.12/site-packages/wheel_stub/wheel.py", line 251, in download_wheel
          report_install_failure(distribution, version, config, exception_context)
        File "/Users/cvaxh/.cache/uv/builds-v0/.tmp6mLMjB/lib/python3.12/site-packages/wheel_stub/error.py", line 67, in report_install_failure
          raise InstallFailedError(
      wheel_stub.error.InstallFailedError:
      *******************************************************************************

      The installation of tensorrt-cu12-libs for version 10.8.0.43 failed.

      This is a special placeholder package which downloads a real wheel package
      from https://pypi.nvidia.com/. If https://pypi.nvidia.com/ is not reachable, we
      cannot download the real wheel file to install.

      You might try installing this package via
      ```
      $ pip install --extra-index-url https://pypi.nvidia.com/ tensorrt-cu12-libs
      ```

      Here is some debug information about your platform to include in any bug
      report:

      Python Version: CPython 3.12.9
      Operating System: Darwin 24.3.0
      CPU Architecture: arm64
      nvidia-smi command not found. Ensure NVIDIA drivers are installed.

      *******************************************************************************


      hint: This usually indicates a problem with the package or the build environment.
  help: `tensorrt-cu12-libs` (v10.8.0.43) was included because `inference-server:cuda` (v0.1.0) depends on `tensorrt-cu12-libs>=10.8.0.43`
```

I also tried adding sys_platform != 'darwin' and sys_platform == 'linux' and it still fails. Note that this only happens if I do not have a uv.lock in my folder. I think I understand that since I dont have a uv.lock, its checking everything, but since I'm on a mac for local dev and a linux machine for remote work, it would be nice if this could work regardless.

I'm not sure if this is a bug or I'm just doing something wrong, please inform me!

Cheers

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.5.31 (e38ac4900 2025-02-12)

### Python version

3.12.9

---

_Label `bug` added by @spkgyk on 2025-02-18 22:58_

---

_Comment by @charliermarsh on 2025-02-19 00:06_

This is an unfortunate consequence of how `tensorrt-cu12` works today. You can track progress in https://github.com/NVIDIA/TensorRT/issues/4352 and https://github.com/wheelnext/wheel-stub/issues/21. The basic issue is: if we don't already have a lockfile, we need to generate one. And to generate one, we have to determine the dependencies for each package you depend on (even if it won't be installed on the current platform, since the lockfile is meant to be used on all platforms). To determine the dependencies for `tensorrt-cu12`, we have to ask that package for its metadata. And the way the package works today, it won't return its dependencies unless you're on a supported platform. So even if you don't need to install it on macOS, `tensorrt-cu12` will error out on a platform like macOS.

---

_Comment by @spkgyk on 2025-02-19 00:27_

Makes sense, thanks for explaining that! 

---

_Closed by @spkgyk on 2025-02-19 00:27_

---
