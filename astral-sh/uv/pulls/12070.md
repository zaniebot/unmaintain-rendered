```yaml
number: 12070
title: "Automatically infer the PyTorch index via `--torch-backend=auto`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - no-build
assignees: []
merged: true
base: main
head: charlie/ltt
created_at: 2025-03-09T01:47:30Z
updated_at: 2025-07-18T23:08:51Z
url: https://github.com/astral-sh/uv/pull/12070
synced_at: 2026-01-10T06:53:01Z
```

# Automatically infer the PyTorch index via `--torch-backend=auto`

---

_Pull request opened by @charliermarsh on 2025-03-09 01:47_

## Summary

This is a prototype that I'm considering shipping under `--preview`, based on [`light-the-torch`](https://github.com/pmeier/light-the-torch).

`light-the-torch` patches pip to pull PyTorch packages from the PyTorch indexes automatically. And, in particular, `light-the-torch` will query the installed CUDA drivers to determine which indexes are compatible with your system.

This PR implements equivalent behavior under `--torch-backend auto`, though you can also set `--torch-backend cpu`, etc. for convenience. When enabled, the registry client will fetch from the appropriate PyTorch index when it sees a package from the PyTorch ecosystem (and ignore any other configured indexes, _unless_ the package is explicitly pinned to a different index).

Right now, this is only implemented in the `uv pip` CLI, since it doesn't quite fit into the lockfile APIs given that it relies on feature detection on the currently-running machine.

## Test Plan

On macOS, you can test this with (e.g.):

```shell
UV_TORCH_BACKEND=auto UV_CUDA_DRIVER_VERSION=450.80.2 cargo run \
  pip install torch --python-platform linux --python-version 3.12
```

On a GPU-enabled EC2 machine:

```shell
ubuntu@ip-172-31-47-149:~/uv$ UV_TORCH_BACKEND=auto cargo run pip install torch -v
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.31s
     Running `target/debug/uv pip install torch -v`
DEBUG uv 0.6.6 (e95ca063b 2025-03-14)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.13.0-linux-x86_64-gnu` at `/home/ubuntu/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.13.0 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: torch
warning: The `--torch-backend` setting is experimental and may change without warning. Pass `--preview` to disable this warning.
DEBUG Detected CUDA driver version from `/sys/module/nvidia/version`: 550.144.3
...
```


---

_Label `no-build` added by @charliermarsh on 2025-03-09 01:53_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-10 20:28_

---

_Review requested from @konstin by @charliermarsh on 2025-03-10 20:28_

---

_Review comment by @konstin on `crates/uv-python/python/get_interpreter_info.py`:420 on 2025-03-10 20:39_

This ties a machine-wide information (`nvidia-smi` output) to each interpreter, even though this information can changes through different operations than an interpreter (updating CUDA vs. updating the Python interpreter binary).

---

_Review comment by @konstin on `crates/uv-python/python/get_interpreter_info.py`:421 on 2025-03-10 20:40_

This could raise an `IndexError`

---

_@konstin reviewed on 2025-03-10 20:41_

---

_Comment by @DEKHTIARJonathan on 2025-03-10 21:42_

@charliermarsh : Adapted from a few different sources - namely conda.
Credit / Code Author: Michael Sarahan

I hope that illustrates my point better - why you need a plugin interface and you don't want to be the person responsible to maintain that :+1: 

```python
# Copyright (C) 2012 Anaconda, Inc
# SPDX-License-Identifier: BSD-3-Clause
"""Detect CUDA version."""

import ctypes
import functools
import itertools
import multiprocessing
import os
import platform
from contextlib import suppress
from dataclasses import dataclass
from typing import Optional


@dataclass()
class CudaVersion:
    version: str
    architectures: list[str]


def cuda_version() -> Optional[CudaVersion]:
    # Do not inherit file descriptors and handles from the parent process.
    # The `fork` start method should be considered unsafe as it can lead to
    # crashes of the subprocess. The `spawn` start method is preferred.
    context = multiprocessing.get_context("spawn")
    queue = context.SimpleQueue()
    # Spawn a subprocess to detect the CUDA version
    detector = context.Process(
        target=_cuda_detector_target,
        args=(queue,),
        name="CUDA driver version detector",
        daemon=True,
    )
    try:
        detector.start()
        detector.join(timeout=60.0)
    finally:
        # Always cleanup the subprocess
        detector.kill()  # requires Python 3.7+

    if queue.empty():
        return None

    result = queue.get()
    if result:
        driver_version, architectures = result.split(";")
        result = CudaVersion(driver_version, architectures.split(","))
    return result


@functools.lru_cache(maxsize=None)
def cached_cuda_version():
    return cuda_version()


def _cuda_detector_target(queue):
    """
    Attempt to detect the version of CUDA present in the operating system in a
    subprocess.

    On Windows and Linux, the CUDA library is installed by the NVIDIA
    driver package, and is typically found in the standard library path,
    rather than with the CUDA SDK (which is optional for running CUDA apps).

    On macOS, the CUDA library is only installed with the CUDA SDK, and
    might not be in the library path.

    Returns: version string with CUDA version first, then a set of unique SM's for the GPUs present in the system
             (e.g., '12.4;8.6,9.0') or None if CUDA is not found.
             The result is put in the queue rather than a return value.
    """
    # Platform-specific libcuda location
    system = platform.system()
    if system == "Darwin":
        lib_filenames = [
            "libcuda.1.dylib",  # check library path first
            "libcuda.dylib",
            "/usr/local/cuda/lib/libcuda.1.dylib",
            "/usr/local/cuda/lib/libcuda.dylib",
        ]
    elif system == "Linux":
        lib_filenames = [
            "libcuda.so",  # check library path first
            "/usr/lib64/nvidia/libcuda.so",  # RHEL/Centos/Fedora
            "/usr/lib/x86_64-linux-gnu/libcuda.so",  # Ubuntu
            "/usr/lib/wsl/lib/libcuda.so",  # WSL
        ]
        # Also add libraries with version suffix `.1`
        lib_filenames = list(
            itertools.chain.from_iterable((f"{lib}.1", lib) for lib in lib_filenames)
        )
    elif system == "Windows":
        bits = platform.architecture()[0].replace("bit", "")  # e.g. "64" or "32"
        lib_filenames = [f"nvcuda{bits}.dll", "nvcuda.dll"]
    else:
        queue.put(None)  # CUDA not available for other operating systems
        return

    # Open library
    if system == "Windows":
        dll = ctypes.windll
    else:
        dll = ctypes.cdll
    for lib_filename in lib_filenames:
        with suppress(Exception):
            libcuda = dll.LoadLibrary(lib_filename)
            break
    else:
        queue.put(None)
        return

    # Empty `CUDA_VISIBLE_DEVICES` can cause `cuInit()` returns `CUDA_ERROR_NO_DEVICE`
    # Invalid `CUDA_VISIBLE_DEVICES` can cause `cuInit()` returns `CUDA_ERROR_INVALID_DEVICE`
    # Unset this environment variable to avoid these errors
    os.environ.pop("CUDA_VISIBLE_DEVICES", None)

    # Get CUDA version
    try:
        cuInit = libcuda.cuInit
        flags = ctypes.c_uint(0)
        ret = cuInit(flags)
        if ret != 0:
            queue.put(None)
            return

        cuDriverGetVersion = libcuda.cuDriverGetVersion
        version_int = ctypes.c_int(0)
        ret = cuDriverGetVersion(ctypes.byref(version_int))
        if ret != 0:
            queue.put(None)
            return

        # Convert version integer to version string
        value = version_int.value
        version_value = f"{value // 1000}.{(value % 1000) // 10}"

        count = ctypes.c_int(0)
        libcuda.cuDeviceGetCount(ctypes.pointer(count))

        architectures = set()
        for device in range(count.value):
            major = ctypes.c_int(0)
            minor = ctypes.c_int(0)
            libcuda.cuDeviceComputeCapability(
                ctypes.pointer(major),
                ctypes.pointer(minor),
                device)
            architectures.add(f"{major.value}.{minor.value}")
        queue.put(f"{version_value};{','.join(architectures)}")
    except Exception:
        queue.put(None)
        return

if __name__ == "__main__":
    print(cuda_version())
```

---

_Review comment by @konstin on `crates/uv-torch/src/lib.rs`:174 on 2025-03-11 12:00_

Can we add this list to some documentation? Reading the high-level overview I didn't realize we were hardcoding a package list.

---

_@konstin reviewed on 2025-03-11 12:09_

---

_Review comment by @geofft on `crates/uv-python/python/get_interpreter_info.py`:420 on 2025-03-11 17:12_

I think we should get this via `/sys/module/nvidia/version`, and also not cache it - it ought to be pretty cheap to do one file read whenever we need it, much faster than actually loading the CUDA libraries and doing stuff. (I don't think there's a good way to get proactively notified if it changes; you can of course invalidate on a reboot but you can also unload/load drivers without a reboot.) I think I've also run into cases where nvidia-smi isn't installed right but the actual kernel driver is fine.

That is to say, I think this logic should move out of the Python interpreter discovery code and into the Rust code at the point where we need it.

Minor point but I want to mention it because the terminology is confusing: the information we're specifically getting here is the driver version, not the CUDA version. PyTorch ships the relevant CUDA runtime (libcudart.so.12 or .11 or whatever) and it doesn't have to match the CUDA version installed systemwide (if any). libcudart, in turn, requires a libcuda.so.1 from either the systemwide driver installation or a "cuda-compat" package if libcudart.so.N is sufficiently newer than the system version of libcuda.so.1. (So you could come up with a scheme where libcuda.so.1 itself is also distributed via e.g. a wheel and so everything is decoupled from the system except the kernel driver, though I don't remember off hand whether NVIDIA's license allows redistributing it. This sort of setup is particularly helpful for containerized environments, where it's annoying that the "driver" installation is split between a kernel driver, which is trivially accessible in the container, and the userspace libcuda.so.1, which requires more effort to bind mount into the container.)

---

_Review comment by @geofft on `crates/uv/tests/it/lock.rs`:15226 on 2025-03-11 17:23_

Is this change fine? (As in, are there real-world users who would have benefited from the hint and are losing it?)

---

_Review comment by @geofft on `docs/reference/cli.md`:5949 on 2025-03-11 17:24_

I wonder if it would be helpful to put the full giant list behind a `<summary>...</summary>`.

---

_Review comment by @geofft on `crates/uv-torch/src/lib.rs`:174 on 2025-03-11 17:26_

Can we generate this by querying the PyTorch indices to see what they have? (Maybe a manually-run script that queries them and updates this list, or an automatically-run integration tests that makes sure this list is in sync with what's currently on their indices?)

Along those lines it would be helpful to have this list somewhere declarative. It might also be helpful to allow user-controlled overrides of this list if the set of packages changes.

---

_@geofft reviewed on 2025-03-11 17:31_

I think this is a great idea.

Would it be worth naming this feature something like `uv-specialized-index` instead of `uv-torch` with an eye to extending it to other libraries in the future? (jaxlib and tensorflow, for instance, have current/popular versions on PyPI, but I think also have their own indees)?

---

_@zanieb reviewed on 2025-03-11 19:42_

---

_Review comment by @zanieb on `docs/reference/cli.md`:5949 on 2025-03-11 19:42_

(Generally non-trivial because this is generated and then rendered via mkdocs)

---

_Comment by @samypr100 on 2025-03-12 01:34_

> I think this is a great idea.
> 
> Would it be worth naming this feature something like `uv-specialized-index` instead of `uv-torch` with an eye to extending it to other libraries in the future? (jaxlib and tensorflow, for instance, have current/popular versions on PyPI, but I think also have their own indees)?

I had a similar thought, I think this is one of many cases. Also considering when such indexes are mirrored or vendored internally. I was thinking what would be the right naming. I know some avenues refers to this as a `suffixed` index, so maybe `uv-suffixed-index`? Same with `--torch-backend`, maybe something more generic of it's intent would be more future proof, such as `--index-suffix`

---

_Comment by @samypr100 on 2025-03-12 01:50_

>  though I don't remember off hand whether NVIDIA's license allows redistributing it

~~iirc this is no longer an issue with the new open source drivers (e.g. `nvidia-driver-{ver}-open`)~~

Nevermind, didn't notice you were referring to CUDA.

> I think we should get this via /sys/module/nvidia/version

💯 In my experience nvidia-smi can also take a long time depending on gpu load. 

Although there multiple locations depending on how (e.g. dkms) and environment (windows, osx) it's installed. For example, WSL 2 its even weirder due to the shared drivers with the host situation. So nvidia-smi might be the most sure-fire low risk way (assuming no issues with install).

---

_Comment by @charliermarsh on 2025-03-12 02:19_

Definitely agree with moving this out of the interpreter query (and possibly reading it from outside `nvidia-smi` -- I need to do some research).

I'm a _little_ wary of trying to brand this as something more general than `torch`, because I'll likely want to reconsider the mechanism and design entirely as we generalize it. So it seems nice to keep it as an experimental `torch`-specific feature, then modify it as we generalize.

---

_@charliermarsh reviewed on 2025-03-15 01:19_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:15226 on 2025-03-15 01:19_

Yeah, I think the hint here is actually wrong.

---

_Review comment by @charliermarsh on `crates/uv-python/python/get_interpreter_info.py`:420 on 2025-03-15 02:16_

Done!

---

_@charliermarsh reviewed on 2025-03-15 02:16_

---

_@charliermarsh reviewed on 2025-03-15 02:16_

---

_Review comment by @charliermarsh on `crates/uv-python/python/get_interpreter_info.py`:421 on 2025-03-15 02:16_

(Removed.)

---

_Review requested from @konstin by @charliermarsh on 2025-03-15 02:21_

---

_Review requested from @geofft by @charliermarsh on 2025-03-15 02:21_

---

_@charliermarsh reviewed on 2025-03-15 02:22_

---

_Review comment by @charliermarsh on `crates/uv-torch/src/lib.rs`:174 on 2025-03-15 02:22_

Unfortunately I don't know that we can... We don't want _all_ packages on these indexes, because they include things like `jinja2`. And in some cases, they include _incomplete_ packages like `markupsafe` (where they only have a few wheels).

---

_Comment by @charliermarsh on 2025-03-15 02:25_

@konstin @geofft -- I believe I've addressed all feedback: we now query from `/sys/module/nvidia/version` and fall back to `nvidia-smi`; and all the accelerator stuff is decoupled from the Python interpreter (and no longer cached).

---

_@konstin approved on 2025-03-17 10:21_

deferring to @geofft for the new detect logic

---

_Review comment by @geofft on `crates/uv-torch/src/accelerator.rs`:72 on 2025-03-18 23:02_

Should this case return an error instead of falling through to `nvidia-smi`?

---

_Review comment by @geofft on `crates/uv-torch/src/accelerator.rs`:90 on 2025-03-18 23:04_

`else {debug!("nvidia-smi returned error {output.status}: {output.stderr}")}` might be nice

---

_@geofft approved on 2025-03-18 23:09_

---

_@charliermarsh reviewed on 2025-03-19 02:29_

---

_Review comment by @charliermarsh on `crates/uv-torch/src/accelerator.rs`:72 on 2025-03-19 02:29_

I'm not confident enough in the format of this one... It seems like it varies across machines.

---

_Merged by @charliermarsh on 2025-03-19 14:37_

---

_Closed by @charliermarsh on 2025-03-19 14:37_

---

_Branch deleted on 2025-03-19 14:37_

---

_Review comment by @coezbek on `crates/uv-torch/src/accelerator.rs`:87 on 2025-07-16 10:15_

On a system with multiple GPUs this line will return multiple driver versions, e.g. on my system:
```
$ nvidia-smi --query-gpu=driver_version --format=csv,noheader
572.60
572.60
```

This will make `uv pip install with --torch-backend=auto` fail with the following error:

```
uv pip install -U "vllm[audio]" --torch-backend=auto
error: after parsing `572.60
`, found `572.60
`, which is not part of a valid version
```

`nvidia-smi` does not respect NVIDIA_VISIBLE_DEVICES, so there is no way from the outside to use `--torch-backend=auto` with two graphics cards at the moment.

Workaround is to run nvidia-smi, identify CUDA version there and run with `--torch-backend=cuXXX` as indicated by `nvidia-smi`.




---

_@coezbek reviewed on 2025-07-16 10:17_

--torch-backend=auto fails for multiple GPU systems which don't have `/sys/module/nvidia/version` or `/proc/driver/nvidia/version` (e.g. WSL)

---

_Comment by @DEKHTIARJonathan on 2025-07-16 16:08_

@charliermarsh I recommend you to use `nvml` - that's what we decided to use for the variant plugin.
It's guaranteed to be present if the driver is installed

Example:
https://github.com/wheelnext/nvidia-variant-provider/blob/dev/nvidia_variant_provider/detect_cuda.py

This is using the Python bindings - but NVML is C library you can directly dlopen. The python example should tell you what functions to call for SM, UMD and KMD ;) 

---

_Comment by @charliermarsh on 2025-07-16 16:16_

Awesome, thanks @DEKHTIARJonathan. I filed an issue here: https://github.com/astral-sh/uv/issues/14664

---

_Comment by @hamzaq2000 on 2025-07-18 22:00_

Tested the following command on a system with a GTX 1080 Ti and CUDA 12.8 driver (what shows in `nvidia-smi`) installed.
```
uv pip install torch --torch-backend=auto --preview
```
Then ran this test script:
```python
import torch
tensor = torch.randn(3, 4, device='cuda')
print(tensor)
```
And got this:
```
  cpu = _conversion_method_template(device=torch.device("cpu"))
/root/env/lib/python3.11/site-packages/torch/cuda/__init__.py:262: UserWarning:
    Found GPU0 NVIDIA GeForce GTX 1080 Ti which is of cuda capability 6.1.
    PyTorch no longer supports this GPU because it is too old.
    The minimum cuda capability supported by this library is 7.5.

  warnings.warn(
/root/env/lib/python3.11/site-packages/torch/cuda/__init__.py:287: UserWarning:
NVIDIA GeForce GTX 1080 Ti with CUDA capability sm_61 is not compatible with the current PyTorch installation.
The current PyTorch install supports CUDA capabilities sm_75 sm_80 sm_86 sm_90 sm_100 sm_120 compute_120.
If you want to use the NVIDIA GeForce GTX 1080 Ti GPU with PyTorch, please check the instructions at https://pytorch.org/get-started/locally/

  warnings.warn(
Traceback (most recent call last):
  File "/root/torch_test.py", line 4, in <module>
    tensor = torch.randn(3, 4, device='cuda')
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
RuntimeError: CUDA error: no kernel image is available for execution on the device
CUDA kernel errors might be asynchronously reported at some other API call, so the stacktrace below might be incorrect.
For debugging consider passing CUDA_LAUNCH_BLOCKING=1
Compile with `TORCH_USE_CUDA_DSA` to enable device-side assertions.
```
I think just looking at the installed CUDA version/driver might not be enough, the CUDA compute capability supported by different torch versions is of relevance as well. In this case, a GTX 1080 Ti has drivers installed that show CUDA 12.8 in `nvidia-smi` but we can't actually use the `cu128` torch wheel, which `--torch-backend=auto` installs, as it doesn't support GPUs with CUDA compute capability < 7.5.

Some relevant discussion I found:
https://discuss.pytorch.org/t/gpu-compute-capability-support-for-each-pytorch-version/62434/4
https://github.com/moi90/pytorch_compute_capabilities

I then also tested without `--torch-backend=auto`, so simply:
```
uv pip install torch
```
This seems to install the `cu126` wheel, and with this the test script worked just fine, meaning the `cu126` wheel has not yet dropped support for CUDA compute capability 6.1 devices.

---

_Comment by @charliermarsh on 2025-07-18 22:33_

Do you mind filing a separate issue? We tend to prefer that over commenting on closed issues or pull requests.

---

_Comment by @DEKHTIARJonathan on 2025-07-18 22:40_

@hamzaq2000 

> a GTX 1080 Ti has drivers installed that show CUDA 12.8 in nvidia-smi but we can't actually use the cu128 torch wheel, which --torch-backend=auto installs, as it doesn't support GPUs with CUDA compute capability < 7.5.

It's not entirely accurate. It's deprecated starting from CUDA 12.8 but not incompatible (otherwise you wouldn't have been to install it).
What happens is that PyTorch stopped building SM < 7.5 with their newer releases.

I don't think @charliermarsh can fix it without having a massive headache of if/else conditions (it's on a per-library/package basis). @hamzaq2000 Just install `CUDA 12.6`, it won't be of any use to you to be on a newer release and you won't stumble into such issues ;) 

---

_Comment by @hamzaq2000 on 2025-07-18 23:08_

@charliermarsh 
> Do you mind filing a separate issue? We tend to prefer that over commenting on closed issues or pull requests.

Certainly, sorry about that! #14742

@DEKHTIARJonathan

Unfortunately I'm not knowledgeable enough to comment on the feasibility of this, so I won't. But I thought it was worth putting out there; hopefully it is fixable and `--torch-backend=auto` can cleanly install the the correct torch wheel from the correct index for all NVIDIA GPUs.

---
