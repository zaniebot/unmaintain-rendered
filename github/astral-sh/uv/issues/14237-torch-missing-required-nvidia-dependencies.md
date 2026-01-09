---
number: 14237
title: torch missing required nvidia dependencies
type: issue
state: open
author: dolfim-ibm
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-06-24T12:22:28Z
updated_at: 2025-07-02T19:19:50Z
url: https://github.com/astral-sh/uv/issues/14237
synced_at: 2026-01-07T13:12:18-06:00
---

# torch missing required nvidia dependencies

---

_Issue opened by @dolfim-ibm on 2025-06-24 12:22_

### Summary

I have the following minimal example pyproject.toml

```toml
[project]
name = "torch-groups"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]

[dependency-groups]
cpu = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]
cu126 = [
  "torch>=2.6.0",
  "torchvision>=0.21.0",
]

[tool.uv]
package = true
conflicts = [
  [
    { group = "cpu" },
    { group = "cu126" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", group = "cpu" },
  { index = "pytorch-cu126", group = "cu126" },
]
torchvision = [
  { index = "pytorch-cpu", group = "cpu" },
  { index = "pytorch-cu126", group = "cu126" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true
```

The expected behavior would be

1. `uv sync` should install the pypi packages
    - for linux x86_64 it should actually install the cuda version
    - for other OS or arch, the pypi packages are cpu-only
2. `uv sync --group cpu` should install from the cpu index
3. `uv sync --group cu126` should install from the cuda 12.6 index


Unfortunately, the `uv.lock` file contains the following

```
[[package]]
name = "torch"
version = "2.7.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version >= '3.12'",
    "python_full_version == '3.11.*'",
    "python_full_version < '3.11'",
]
dependencies = [
    { name = "filelock", marker = "(extra == 'group-12-torch-groups-cpu' and extra == 'group-12-torch-groups-cu126') or (extra != 'group-12-torch-groups-cpu' and extra != 'group-12-torch-groups-cu126')" },
    { name = "fsspec", marker = "(extra == 'group-12-torch-groups-cpu' and extra == 'group-12-torch-groups-cu126') or (extra != 'group-12-torch-groups-cpu' and extra != 'group-12-torch-groups-cu126')" },
    { name = "jinja2", marker = "(extra == 'group-12-torch-groups-cpu' and extra == 'group-12-torch-groups-cu126') or (extra != 'group-12-torch-groups-cpu' and extra != 'group-12-torch-groups-cu126')" },
    { name = "networkx", version = "3.4.2", source = { registry = "https://pypi.org/simple" }, marker = "(python_full_version < '3.11' and extra != 'group-12-torch-groups-cpu' and extra != 'group-12-torch-groups-cu126') or (extra == 'group-12-torch-groups-cpu' and extra == 'group-12-torch-groups-cu126')" },
    { name = "networkx", version = "3.5", source = { registry = "https://pypi.org/simple" }, marker = "(python_full_version >= '3.11' and extra != 'group-12-torch-groups-cpu' and extra != 'group-12-torch-groups-cu126') or (extra == 'group-12-torch-groups-cpu' and extra == 'group-12-torch-groups-cu126')" },
    { name = "setuptools", marker = "(python_full_version >= '3.12' and extra != 'group-12-torch-groups-cpu' and extra != 'group-12-torch-groups-cu126') or (extra == 'group-12-torch-groups-cpu' and extra == 'group-12-torch-groups-cu126')" },
    { name = "sympy", marker = "(extra == 'group-12-torch-groups-cpu' and extra == 'group-12-torch-groups-cu126') or (extra != 'group-12-torch-groups-cpu' and extra != 'group-12-torch-groups-cu126')" },
    { name = "typing-extensions", marker = "(extra == 'group-12-torch-groups-cpu' and extra == 'group-12-torch-groups-cu126') or (extra != 'group-12-torch-groups-cpu' and extra != 'group-12-torch-groups-cu126')" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/6a/27/2e06cb52adf89fe6e020963529d17ed51532fc73c1e6d1b18420ef03338c/torch-2.7.1-cp310-cp310-manylinux_2_28_aarch64.whl", hash = "sha256:a103b5d782af5bd119b81dbcc7ffc6fa09904c423ff8db397a1e6ea8fd71508f", size = 99089441, upload-time = "2025-06-04T17:38:48.268Z" },
    { url = "https://files.pythonhosted.org/packages/0a/7c/0a5b3aee977596459ec45be2220370fde8e017f651fecc40522fd478cb1e/torch-2.7.1-cp310-cp310-manylinux_2_28_x86_64.whl", hash = "sha256:fe955951bdf32d182ee8ead6c3186ad54781492bf03d547d31771a01b3d6fb7d", size = 821154516, upload-time = "2025-06-04T17:36:28.556Z" },
    { url = "https://files.pythonhosted.org/packages/f9/91/3d709cfc5e15995fb3fe7a6b564ce42280d3a55676dad672205e94f34ac9/torch-2.7.1-cp310-cp310-win_amd64.whl", hash = "sha256:885453d6fba67d9991132143bf7fa06b79b24352f4506fd4d10b309f53454162", size = 216093147, upload-time = "2025-06-04T17:39:38.132Z" },
    { url = "https://files.pythonhosted.org/packages/92/f6/5da3918414e07da9866ecb9330fe6ffdebe15cb9a4c5ada7d4b6e0a6654d/torch-2.7.1-cp310-none-macosx_11_0_arm64.whl", hash = "sha256:d72acfdb86cee2a32c0ce0101606f3758f0d8bb5f8f31e7920dc2809e963aa7c", size = 68630914, upload-time = "2025-06-04T17:39:31.162Z" },
    { url = "https://files.pythonhosted.org/packages/11/56/2eae3494e3d375533034a8e8cf0ba163363e996d85f0629441fa9d9843fe/torch-2.7.1-cp311-cp311-manylinux_2_28_aarch64.whl", hash = "sha256:236f501f2e383f1cb861337bdf057712182f910f10aeaf509065d54d339e49b2", size = 99093039, upload-time = "2025-06-04T17:39:06.963Z" },
    { url = "https://files.pythonhosted.org/packages/e5/94/34b80bd172d0072c9979708ccd279c2da2f55c3ef318eceec276ab9544a4/torch-2.7.1-cp311-cp311-manylinux_2_28_x86_64.whl", hash = "sha256:06eea61f859436622e78dd0cdd51dbc8f8c6d76917a9cf0555a333f9eac31ec1", size = 821174704, upload-time = "2025-06-04T17:37:03.799Z" },
    { url = "https://files.pythonhosted.org/packages/50/9e/acf04ff375b0b49a45511c55d188bcea5c942da2aaf293096676110086d1/torch-2.7.1-cp311-cp311-win_amd64.whl", hash = "sha256:8273145a2e0a3c6f9fd2ac36762d6ee89c26d430e612b95a99885df083b04e52", size = 216095937, upload-time = "2025-06-04T17:39:24.83Z" },
    { url = "https://files.pythonhosted.org/packages/5b/2b/d36d57c66ff031f93b4fa432e86802f84991477e522adcdffd314454326b/torch-2.7.1-cp311-none-macosx_11_0_arm64.whl", hash = "sha256:aea4fc1bf433d12843eb2c6b2204861f43d8364597697074c8d38ae2507f8730", size = 68640034, upload-time = "2025-06-04T17:39:17.989Z" },
    { url = "https://files.pythonhosted.org/packages/87/93/fb505a5022a2e908d81fe9a5e0aa84c86c0d5f408173be71c6018836f34e/torch-2.7.1-cp312-cp312-manylinux_2_28_aarch64.whl", hash = "sha256:27ea1e518df4c9de73af7e8a720770f3628e7f667280bce2be7a16292697e3fa", size = 98948276, upload-time = "2025-06-04T17:39:12.852Z" },
    { url = "https://files.pythonhosted.org/packages/56/7e/67c3fe2b8c33f40af06326a3d6ae7776b3e3a01daa8f71d125d78594d874/torch-2.7.1-cp312-cp312-manylinux_2_28_x86_64.whl", hash = "sha256:c33360cfc2edd976c2633b3b66c769bdcbbf0e0b6550606d188431c81e7dd1fc", size = 821025792, upload-time = "2025-06-04T17:34:58.747Z" },
    { url = "https://files.pythonhosted.org/packages/a1/37/a37495502bc7a23bf34f89584fa5a78e25bae7b8da513bc1b8f97afb7009/torch-2.7.1-cp312-cp312-win_amd64.whl", hash = "sha256:d8bf6e1856ddd1807e79dc57e54d3335f2b62e6f316ed13ed3ecfe1fc1df3d8b", size = 216050349, upload-time = "2025-06-04T17:38:59.709Z" },
    { url = "https://files.pythonhosted.org/packages/3a/60/04b77281c730bb13460628e518c52721257814ac6c298acd25757f6a175c/torch-2.7.1-cp312-none-macosx_11_0_arm64.whl", hash = "sha256:787687087412c4bd68d315e39bc1223f08aae1d16a9e9771d95eabbb04ae98fb", size = 68645146, upload-time = "2025-06-04T17:38:52.97Z" },
    { url = "https://files.pythonhosted.org/packages/66/81/e48c9edb655ee8eb8c2a6026abdb6f8d2146abd1f150979ede807bb75dcb/torch-2.7.1-cp313-cp313-manylinux_2_28_aarch64.whl", hash = "sha256:03563603d931e70722dce0e11999d53aa80a375a3d78e6b39b9f6805ea0a8d28", size = 98946649, upload-time = "2025-06-04T17:38:43.031Z" },
    { url = "https://files.pythonhosted.org/packages/3a/24/efe2f520d75274fc06b695c616415a1e8a1021d87a13c68ff9dce733d088/torch-2.7.1-cp313-cp313-manylinux_2_28_x86_64.whl", hash = "sha256:d632f5417b6980f61404a125b999ca6ebd0b8b4bbdbb5fbbba44374ab619a412", size = 821033192, upload-time = "2025-06-04T17:38:09.146Z" },
    { url = "https://files.pythonhosted.org/packages/dd/d9/9c24d230333ff4e9b6807274f6f8d52a864210b52ec794c5def7925f4495/torch-2.7.1-cp313-cp313-win_amd64.whl", hash = "sha256:23660443e13995ee93e3d844786701ea4ca69f337027b05182f5ba053ce43b38", size = 216055668, upload-time = "2025-06-04T17:38:36.253Z" },
    { url = "https://files.pythonhosted.org/packages/95/bf/e086ee36ddcef9299f6e708d3b6c8487c1651787bb9ee2939eb2a7f74911/torch-2.7.1-cp313-cp313t-macosx_14_0_arm64.whl", hash = "sha256:0da4f4dba9f65d0d203794e619fe7ca3247a55ffdcbd17ae8fb83c8b2dc9b585", size = 68925988, upload-time = "2025-06-04T17:38:29.273Z" },
    { url = "https://files.pythonhosted.org/packages/69/6a/67090dcfe1cf9048448b31555af6efb149f7afa0a310a366adbdada32105/torch-2.7.1-cp313-cp313t-manylinux_2_28_aarch64.whl", hash = "sha256:e08d7e6f21a617fe38eeb46dd2213ded43f27c072e9165dc27300c9ef9570934", size = 99028857, upload-time = "2025-06-04T17:37:50.956Z" },
    { url = "https://files.pythonhosted.org/packages/90/1c/48b988870823d1cc381f15ec4e70ed3d65e043f43f919329b0045ae83529/torch-2.7.1-cp313-cp313t-manylinux_2_28_x86_64.whl", hash = "sha256:30207f672328a42df4f2174b8f426f354b2baa0b7cca3a0adb3d6ab5daf00dc8", size = 821098066, upload-time = "2025-06-04T17:37:33.939Z" },
    { url = "https://files.pythonhosted.org/packages/7b/eb/10050d61c9d5140c5dc04a89ed3257ef1a6b93e49dd91b95363d757071e0/torch-2.7.1-cp313-cp313t-win_amd64.whl", hash = "sha256:79042feca1c634aaf6603fe6feea8c6b30dfa140a6bbc0b973e2260c7e79a22e", size = 216336310, upload-time = "2025-06-04T17:36:09.862Z" },
    { url = "https://files.pythonhosted.org/packages/b1/29/beb45cdf5c4fc3ebe282bf5eafc8dfd925ead7299b3c97491900fe5ed844/torch-2.7.1-cp313-none-macosx_11_0_arm64.whl", hash = "sha256:988b0cbc4333618a1056d2ebad9eb10089637b659eb645434d0809d8d937b946", size = 68645708, upload-time = "2025-06-04T17:34:39.852Z" },
]
```

The **important missing piece** is that `dependencies` doesn't contain the `nvidia-` dependencies.

As a result, when I install with `uv sync` (no group) in linux x86_64:
1. uv installs the wheel from https://pypi.org/simple, which is linking to libcublas.so, etc 
2. but it doesn't install the nvidia-cuda packages

At runtime I get the following error

```
Traceback (most recent call last):
  File "/opt/app-root/lib64/python3.12/site-packages/torch/__init__.py", line 312, in _load_global_deps
    ctypes.CDLL(global_deps_lib_path, mode=ctypes.RTLD_GLOBAL)
  File "/usr/lib64/python3.12/ctypes/__init__.py", line 379, in __init__
    self._handle = _dlopen(self._name, mode)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^
OSError: libcudart.so.12: cannot open shared object file: No such file or directory
```

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.14 (e7f596711 2025-06-23)

### Python version

_No response_

---

_Label `bug` added by @dolfim-ibm on 2025-06-24 12:22_

---

_Label `great writeup` added by @konstin on 2025-06-24 13:12_

---

_Comment by @konstin on 2025-06-24 13:41_

I've minimized the example by removing torchvision, and confirmed the METADATA (specifically the `Requires-Dist`) is consistent for torch 2.7.1 on PyPI.

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["torch>=2.6.0"]

[dependency-groups]
cpu = ["torch>=2.6.0"]
cu126 = ["torch>=2.6.0"]

[tool.uv]
conflicts = [
  [
    { group = "cpu" },
    { group = "cu126" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", group = "cpu" },
  { index = "pytorch-cu126", group = "cu126" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true
```

---

_Assigned to @BurntSushi by @BurntSushi on 2025-06-26 15:57_

---

_Comment by @BurntSushi on 2025-07-01 19:57_

What I think I'm seeing here is that while the wheel metadata may be consistent across wheels published in the same index, it seems to _not_ match wheel metadata for different local versions in the pytorch index. I'm not sure if there is expectation for it to be so, but for example:

```
$ curl -sL 'https://download.pytorch.org/whl/cpu/torch-2.7.1%2Bcpu-cp312-cp312-manylinux_2_28_aarch64.whl.metadata' | rg Requires-Dist
Requires-Dist: filelock
Requires-Dist: typing-extensions>=4.10.0
Requires-Dist: setuptools; python_version >= "3.12"
Requires-Dist: sympy>=1.13.3
Requires-Dist: networkx
Requires-Dist: jinja2
Requires-Dist: fsspec
Requires-Dist: optree>=0.13.0; extra == "optree"
Requires-Dist: opt-einsum>=3.3; extra == "opt-einsum"

$ curl -sL 'https://files.pythonhosted.org/packages/87/93/fb505a5022a2e908d81fe9a5e0aa84c86c0d5f408173be71c6018836f34e/torch-2.7.1-cp312-cp312-manylinux_2_28_aarch64.whl.metadata' | rg Requires-Dist
Requires-Dist: filelock
Requires-Dist: typing-extensions>=4.10.0
Requires-Dist: setuptools; python_version >= "3.12"
Requires-Dist: sympy>=1.13.3
Requires-Dist: networkx
Requires-Dist: jinja2
Requires-Dist: fsspec
Requires-Dist: nvidia-cuda-nvrtc-cu12==12.6.77; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cuda-runtime-cu12==12.6.77; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cuda-cupti-cu12==12.6.80; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cudnn-cu12==9.5.1.17; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cublas-cu12==12.6.4.1; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cufft-cu12==11.3.0.4; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-curand-cu12==10.3.7.77; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cusolver-cu12==11.7.1.2; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cusparse-cu12==12.5.4.2; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cusparselt-cu12==0.6.3; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-nccl-cu12==2.26.2; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-nvtx-cu12==12.6.77; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-nvjitlink-cu12==12.6.85; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cufile-cu12==1.11.1.6; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: triton==3.3.1; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: optree>=0.13.0; extra == "optree"
Requires-Dist: opt-einsum>=3.3; extra == "opt-einsum"
```

What I _think_ is happening here is that the metadata from the wheels in the pytorch index is somehow being used for the metadata for the torch pypi package. I haven't quite figured out why that's happening yet (and whether it's even related to conflict markers). However, I can at least confirm that by the time we build the dependency graph and before most of the conflict marker simplification is performed, we've already lost the nvidia dependencies here for the PyPI torch package.

My next step here is to figure out exactly why the right metadata isn't being used here and whether that can be fixed.

---

_Comment by @BurntSushi on 2025-07-02 18:44_

After a bit more investigation, what ends up happening here is that `cpu = ["torch>=2.6.0"]` from `pyproject.toml` ends up introducing a dependency on `torch 2.7.1` _and_ `torch 2.7.1+cpu` from `pytorch.org`, where the former is used on darwin-only and the latter is used otherwise. But there is _also_ a `torch 2.7.1` dependency from PyPI introduced by `dependencies = ["torch>=2.6.0"]`.

As shown in my previous comment, the wheels from `torch 2.7.1` on the pytorch.org and PyPI index have _different metadata_. I don't know if that's intended or not, but it sure seems strange.

Normally, uv requires that wheels published for the same version of a package have the same metadata. Historically, I've always conceived of that as wheels for the same _index_. But it seems like, at least today, we require that to be true for all wheels of a single version of a package across all active indexes during resolution. Off the top of my head, it's not totally clear what would be needed to lift that assumption (or if it's practical to do so). For example, we key all of our wheel metadata off of [`VersionId`](https://github.com/astral-sh/uv/blob/3774a656d7c864370408b8c253909af3b83296d5/crates/uv-distribution-types/src/id.rs#L42-L49), which is in turn only a pair of package name and version. It doesn't include the index URL. So that _at minimum_, I think, would need to change.

cc @konstin to check my understanding here.

---

_Comment by @konstin on 2025-07-02 19:19_

That matches my understanding.

---

_Referenced in [NovaSky-AI/SkyRL#341](../../NovaSky-AI/SkyRL/pulls/341.md) on 2025-09-29 02:22_

---
