```yaml
number: 15521
title: "UV lock contains package without artifacts (when installing `cuml-cu12`, `ray[data]`, `torch`)"
type: issue
state: closed
author: pimdh
labels:
  - bug
assignees: []
created_at: 2025-08-25T22:39:34Z
updated_at: 2025-08-27T20:59:01Z
url: https://github.com/astral-sh/uv/issues/15521
synced_at: 2026-01-10T03:23:54Z
```

# UV lock contains package without artifacts (when installing `cuml-cu12`, `ray[data]`, `torch`)

---

_Issue opened by @pimdh on 2025-08-25 22:39_

### Summary

With the `pyproject.toml` file
```toml
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "cuml-cu12==25.8",
  "ray[data]==2.47.1",
  "torch==2.6.0+cu124",
]

[[tool.uv.index]]
url = "https://download.pytorch.org/whl/cu124"
```

The command `uv lock` results in a `uv.lock` which contains one package with artifacts and one package without any artifact
```toml
[[package]]
name = "nvidia-cublas-cu12"
version = "12.4.5.8"
source = { registry = "https://download.pytorch.org/whl/cu124" }
resolution-markers = [
    "platform_machine == 'aarch64'",
    "platform_machine == 'x86_64' and sys_platform != 'darwin'",
    "platform_machine != 'aarch64' and platform_machine != 'x86_64'",
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu124/nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_aarch64.whl", hash = "sha256:0f8aa1706812e00b9f19dfe0cdb3999b092ccb8ca168c0db5b8ea712456fd9b3" },
    { url = "https://download.pytorch.org/whl/cu124/nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_x86_64.whl", hash = "sha256:2fc8da60df463fdefa81e323eef2e36489e1c94335b5358bcb38360adf75ac9b" },
    { url = "https://download.pytorch.org/whl/cu124/nvidia_cublas_cu12-12.4.5.8-py3-none-win_amd64.whl", hash = "sha256:5a796786da89203a0657eda402bcdcec6180254a8ac22d72213abc42069522dc" },
]

[[package]]
name = "nvidia-cublas-cu12"
version = "12.9.1.4"
source = { registry = "https://download.pytorch.org/whl/cu124" }
resolution-markers = [
    "platform_machine == 'x86_64' and sys_platform == 'darwin'",
]
```
On that index, for the latter no corresponding package seems to exist.

The expected behaviour would be to only resolve for markers for which artifacts exist.

This issue also arises in `pylock.toml` files generated with `uv export` (using #14783), which subsequently raises errors in PEX, with which I'm trying to consume said lock files.

### Platform

Linux 6.8.0-1027-gke x86_64 GNU/Linux

### Version

uv 0.8.13 (9b8d6989d 2025-08-25)

### Python version

_No response_

---

_Label `bug` added by @pimdh on 2025-08-25 22:39_

---

_Comment by @charliermarsh on 2025-08-25 23:56_

I think that might be a sign that this isn't installable on macOS. The resolver should probably error entirely?

---

_Comment by @pimdh on 2025-08-26 00:08_

Wouldn't it be better to exclude the macOS marked package? If I only install torch, with
```toml
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  # "cuml-cu12==25.8",
  # "ray[data]==2.47.1",
  "torch==2.6.0+cu124",
]

[[tool.uv.index]]
url = "https://download.pytorch.org/whl/cu124"
```
I only get the linux version of `cublas`:
```toml
[[package]]
name = "nvidia-cublas-cu12"
version = "12.4.5.8"
source = { registry = "https://download.pytorch.org/whl/cu124" }
wheels = [
    { url = "https://download.pytorch.org/whl/cu124/nvidia_cublas_cu12-12.4.5.8-py3-none-manylinux2014_x86_64.whl", hash = "sha256:2fc8da60df463fdefa81e323eef2e36489e1c94335b5358bcb38360adf75ac9b" },
]
```
this seems like the desired behaviour. This can not be resolved for macOS, so it's not included in any package.
The same happens when I install `cuml-cu12` or both `torch` and `cuml-cu12`.

So why does it resolve with differently happen when I include `ray[data]`? That itself doesn't depend on `nvidia-cublas-cu12` at all, not even transitively.

---

_Comment by @pimdh on 2025-08-26 00:15_

For context, this appears to be happening due the following [requirements](https://github.com/ray-project/ray/blob/releases/2.47.1/python/requirements.txt) of `ray`:
```
# pyarrow 18 causes macos build failures.
# See https://github.com/ray-project/ray/pull/48300
pyarrow >= 9.0.0
pyarrow <18; sys_platform == "darwin" and platform_machine == "x86_64"
```

---

_Comment by @konstin on 2025-08-26 09:42_

By looking at the lockfile, it's clearly nonsense to produce an entry that can never be installed, but by the logic of the universal resolver, this is supported: Packages without source distributions only support a subset of platforms, so we can only produce a resolution for that limited set of platforms. Instead, we error on installation, saying that the platform isn't supported.

For packages where different versions support different platforms, where you want to exclude a platform altogether or you want a version that supports a specific platform, you can define `tool.uv-environment` (https://docs.astral.sh/uv/concepts/resolution/#limited-resolution-environments) or `tool.uv-required-environment` (https://docs.astral.sh/uv/concepts/resolution/#required-environments). For your case this would be the following, which makes the empty entry go away.

```toml
[tool.uv]
environments = ["sys_platform == 'linux'"]
```



---

_Comment by @pimdh on 2025-08-27 07:42_

Okay thanks. This seems consistent with the `pylock.toml` [spec](https://packaging.python.org/en/latest/specifications/pylock-toml/#packages-wheels), which, afaict, does not state that an artifact must be available.

> [[packages.wheels]]
> Type: array of tables
> 
> Required?: no; mutually-exclusive with [[packages.vcs]](https://packaging.python.org/en/latest/specifications/pylock-toml/#pylock-packages-vcs), [[packages.directory]](https://packaging.python.org/en/latest/specifications/pylock-toml/#pylock-packages-directory), and [[packages.archive]](https://packaging.python.org/en/latest/specifications/pylock-toml/#pylock-packages-archive)

I'll see if this can be accommodated when consuming the lockfiles.



---

_Closed by @pimdh on 2025-08-27 07:42_

---

_Comment by @jsirois on 2025-08-27 20:47_

I've fixed Pex to deal with packages with no artifacts, but I think this is definitely a spec weakness. As an internal detail in `uv.lock` it makes total sense - uv code can deal with its own format as it sees fit. As an external detail in `pylock.toml` it seems sub-optimal if only because the spec is sub-optimal and, IIUC you can spell uninstallable for the current platform either by doing what uv does now - emit the package with no artifacts, or by simply not emitting that package at all in the lock. IOW the spec is under-specified here for, from what I can tell, is no good reason. It seems like just a confusion inducing unnecessary degree of freedom. All installers must handle both forms of absence.

---
