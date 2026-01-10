---
number: 8746
title: "`uv add` fails on `torch` \"no wheel for current platform\" Docker Linux on Mac ARM"
type: issue
state: closed
author: bepuca
labels:
  - question
assignees: []
created_at: 2024-11-01T07:01:57Z
updated_at: 2025-08-27T04:05:29Z
url: https://github.com/astral-sh/uv/issues/8746
synced_at: 2026-01-10T01:24:32Z
---

# `uv add` fails on `torch` "no wheel for current platform" Docker Linux on Mac ARM

---

_Issue opened by @bepuca on 2024-11-01 07:01_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

The situation is the following:
- I work on workloads that usually require GPUs. The problem is these machines are expensive (usually use VMs), so I try to do as much as possible on my laptop, a Mac with Apple Sillicon.
- I collaborate with other colleagues who do not have Mac.
- For these reasons, we leverage [devcontainers](https://code.visualstudio.com/docs/devcontainers/containers) to ensure we all operate on the same environment (a linux one).

Now, because of these reasons, I need to install Pytorch + CUDA in these environments, even if in many of the computers it is used, there is no GPU. Ideally, there will be a mechanism for installing CUDA version when GPU is available (or based on env var) and cpu version otherwise, but this is another story.

The problem I have now is I cannot seem to add pytorch+cuda to the project when running it inside a Docker container running linux ARM inside a Mac with Apple Sillicon. But when I do `uv pip install torch --index-url https://download.pytorch.org/whl/cu121`, it works. It installs and can import normally. I would expect, then, to be able to do `uv add torch` and work the same. I know Pytorch installations are a beast of their own, so maybe I am missing something. In that case, I'd love to understand.

## Minimal Example

The file structure looks like this:
```text
.
├── .devcontainer
|   ├── Dockerfile
│   └── devcontainer.json
├── pyproject.toml
└──.python_version
```

**Dockerfile**
```Dockerfile
FROM mcr.microsoft.com/devcontainers/base:jammy

# Install uv
COPY --from=ghcr.io/astral-sh/uv:0.4.29 /uv /uvx /bin/
```

**devcontainer.json**
```jsonc
{
    "name": "minimal",
    "build": { "dockerfile": "Dockerfile" },
    "workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}",
    "remoteEnv": {
        // Ensure venv is always first in PATH
        "PATH": "${containerWorkspaceFolder}/.venv/bin:${containerEnv:PATH}",
    },
    "remoteUser": "vscode",
}
```

**pyproject.toml**
```
[project]
name = "minimal"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
]

[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch-cu121" }
```

**.python_version**
```text
3.11
```

To reproduce you will need:
- A Mac with AppleSillicon
- VSCode with the [devcontainer extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) installed
- Docker in your machine

Then, open the command palette of VSCode (if not directly prompted) and "reopen in container".

Now, when I try to do in the terminal:
```shell
$uv add torch
Resolved 24 packages in 396ms
error: Distribution `torch==2.5.1+cu121 @ registry+https://download.pytorch.org/whl/cu121` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

But if I use `uv pip install torch --index-url https://download.pytorch.org/whl/cu121` it succeeds without issue.

Am I doing something wrong? Can I fix it somehow? I'd really like to port the project to `uv` but this is a big blocker. An working but less appealing alternative, is if I can have a solution for the problem described in #8745 (assuming then I can install pytorch without issues).

---

_Renamed from "`uv add` fails on `torch` saying no wheel for current platform" to "`uv add` fails on `torch` "no wheel for current platform" Docker on Mac ARM" by @bepuca on 2024-11-01 07:04_

---

_Comment by @FishAlchemist on 2024-11-01 07:26_

Let me just say up front that I'm typing this on my phone, so I'll keep it short.

PyTorch Index appears to have never offered pre-built wheels for macOS, which means UV cannot locate Mac-compatible PyTorch packages from it.
(PyTorch wheels for macOS are on PyPI, not on PyTorch Index.)

You need to use PEP 508 to prevent UV from finding wheels from PyTorch Index.

You can refer to https://github.com/astral-sh/uv/pull/6523, or check the document for the explanation of PEP 508.
https://docs.astral.sh/uv/concepts/dependencies/#dependency-specifiers-pep-508


---

_Comment by @bepuca on 2024-11-01 08:59_

Thank you very much for the response. I think is a bit more tricky than that. I did find the issue with the docs you linked, and this is where I based my example from.

The issue is that **the platform is Linux** because all is happening inside a Linux Docker container:

```shell
$ uv run python -c "import platform; print(platform.system())"
Linux
```

Therefore, the wheels I would expect to be found are **arm linux** ones, not macOS. Looking through the wheels in the torch index, I am not sure they offer a cuda version for aarch64. If that is the case, then how come pip install does find something and install it? I manage to do what I am describing with Poetry and I have a hard time understanding the difference.

I realize this might be going more in the direction on me having a flawed understanding rather than anything wrong per se with uv. Thus I understand if you want to close the issue.

That being said, I read multiple times the instructions you linked and failed to succeed. If I manage to make this work, I'd be happy to contribute there too so others don't struggle.

---

_Renamed from "`uv add` fails on `torch` "no wheel for current platform" Docker on Mac ARM" to "`uv add` fails on `torch` "no wheel for current platform" Docker Linux on Mac ARM" by @bepuca on 2024-11-01 09:09_

---

_Comment by @FishAlchemist on 2024-11-01 10:06_

When using Linux aarch64, it might be necessary to specify the version. It is unclear to me why UV would select a newer version that is incompatible with the platform.
While ``uv pip install`` picks the correct old version, ``uv add`` select an unavailable newer one.
![image](https://github.com/user-attachments/assets/3dff4049-487a-4db2-a83a-9dd6ee83f79a)

# There are only these wheels on PyTorch Index (cu121)
![image](https://github.com/user-attachments/assets/37256acc-22ce-4b47-a3e8-414dfdfd5ec1)


---

_Comment by @FishAlchemist on 2024-11-01 10:37_

https://download.pytorch.org/whl/cu124/torch
**Supplement**: By choosing CUDA 12.4, you should be able to install 2.4.0, 2.4.1, 2.5.0, and 2.5.1.

---

_Comment by @bepuca on 2024-11-01 10:46_

You are correct, when I do so, `uv add` still fails but `uv pip` would do the following:

```shell
$ uv pip install torch --index-url https://download.pytorch.org/whl/cu124 --python-platform aarch64-manylinux_2_31 --dry-run

Resolved 9 packages in 4.72s
Would download 9 packages
Would install 9 packages
 + filelock==3.13.1
 + fsspec==2024.2.0
 + jinja2==3.1.3
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.13.1
 + torch==2.5.1
 + typing-extensions==4.9.0
```

But then I understand in this circumstance uv would be expected to have the same behavior?

---

_Comment by @FishAlchemist on 2024-11-01 10:56_

So, lockfile doesn't work on Linux arch64, right?
(Waiting for a response from the maintenance code)
**pyproject.toml**
```
...
requires-python = ">=3.11"
dependencies = [
    "torch==2.5.1 ; platform_system == 'Darwin'",
    "torch==2.5.1+cu124 ; platform_system != 'Darwin'",
]

[tool.uv.sources]
torch = [{ index = "pytorch-cu124", marker = "platform_system != 'Darwin'" }]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```
**uv.lock**
```
name = "torch"
version = "2.5.1+cu124"
source = { registry = "https://download.pytorch.org/whl/cu124" }
resolution-markers = [
    "python_full_version < '3.12' and platform_system != 'Darwin'",
    "python_full_version >= '3.12' and platform_system != 'Darwin'",
]

wheels = [
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-linux_x86_64.whl", hash = "sha256:6b2966ede9affe2fd69e0765691ca723ec870e0c34c7761f4d5b8e318383fdaf" },
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-win_amd64.whl", hash = "sha256:6c8a7003ef1327479ede284b6e5ab3527d3900c2b2d401af15bcc50f2245a59f" },
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-linux_x86_64.whl", hash = "sha256:bf6484bfe5bc4f92a4a1a1bf553041505e19a911f717065330eb061afe0e14d7" },
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-win_amd64.whl", hash = "sha256:3c3f705fb125edbd77f9579fa11a138c56af8968a10fc95834cdd9fdf4f1f1a6" },
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp313-cp313-linux_x86_64.whl", hash = "sha256:e9bebf91ede89267577911da4b0709ac6113a0cff6a1c2202c046b1ec2a51601" },
]
```

---

_Comment by @charliermarsh on 2024-11-01 13:23_

This is a manifestation of https://github.com/astral-sh/uv/issues/5182. I described the problem in more detail here: https://github.com/astral-sh/uv/issues/8536#issuecomment-2436123242. But it's most difficult with PyTorch, where there's no source distribution, and the wheel version tags differ across platforms. I think what @FishAlchemist has is on the right track. What's the issue with that solution? (I don't think PyTorch publishes ARM Linux wheels, at least I don't se them on https://download.pytorch.org/whl/torch/.)

For reference, `uv pip install torch --index-url https://download.pytorch.org/whl/cu121` may not even be giving you the CUDA-accelerated PyTorch. You'd have to look at the selected local version tag.

---

_Label `question` added by @charliermarsh on 2024-11-01 13:23_

---

_Comment by @FishAlchemist on 2024-11-01 14:04_

@charliermarsh 
Since these wheels exist, even though they're not CUDA versions, there should still be supported, right? Or is this not a standard format, making it difficult to search for?
![image](https://github.com/user-attachments/assets/58b45de0-8ec4-40a3-ac10-e5f3e432d331)
Edit(Record):
requires-python = ">=3.11"
From PyPI:
torch-2.5.0-cp311-cp311-manylinux2014_aarch64.whl
torch-2.5.0-cp312-cp312-manylinux2014_aarch64.whl
https://github.com/pytorch/pytorch/blob/main/RELEASE.md#operating-systems
![image](https://github.com/user-attachments/assets/077b05c8-40f2-47cb-bf8f-6cadda613b5b)

----------
@bepuca 
By the way, I've only found NGC supporting it online.
https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch

---

_Comment by @bepuca on 2024-11-01 17:25_

Alright, I am a bit confused with the whole thing as I get a bit lost on the details of the wheels and the implications. What in the end, understood, is that somehow the wheels for aarch64 are a bit obscure and using indexes for them makes it hard or impossible for uv to properly resolve. Then, what seems to solve my problem is this:

`pyproject.toml`
```toml
[project]
name = "minimal"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "torch>=2.5.1",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cu124", marker = "platform_machine == 'x86_64'" }
]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

I have not double checked this does install the cuda version properly in x86_64 but I think it will work. What I do not understand is why like this the wheel can be resolved. In any case, it does not seem PyTorch releases CUDA compatible wheels for aarch64 anyway. Not sure if this is the expected way of how things should work or not, but if it is, I am happy with the result as I can work with this. Thank you very much for all the support.

---

_Comment by @charliermarsh on 2024-11-01 18:49_

Ahh I missed that there are aarch64 wheels way down there. I can post an example of how to get this working.

---

_Comment by @charliermarsh on 2024-11-01 19:07_

So the `pyproject.toml` you have above looks right. If you inspect the lockfile:

```toml
[[package]]
name = "torch"
version = "2.5.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.12' and platform_machine != 'x86_64'",
    "python_full_version >= '3.12' and platform_machine != 'x86_64'",
]
dependencies = [
    { name = "filelock", marker = "platform_machine != 'x86_64'" },
    { name = "fsspec", marker = "platform_machine != 'x86_64'" },
    { name = "jinja2", marker = "platform_machine != 'x86_64'" },
    { name = "networkx", marker = "platform_machine != 'x86_64'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and platform_machine != 'x86_64'" },
    { name = "sympy", marker = "platform_machine != 'x86_64'" },
    { name = "typing-extensions", marker = "platform_machine != 'x86_64'" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/d1/35/e8b2daf02ce933e4518e6f5682c72fd0ed66c15910ea1fb4168f442b71c4/torch-2.5.1-cp311-cp311-manylinux1_x86_64.whl", hash = "sha256:de5b7d6740c4b636ef4db92be922f0edc425b65ed78c5076c43c42d362a45457", size = 906474467 },
    { url = "https://files.pythonhosted.org/packages/40/04/bd91593a4ca178ece93ca55f27e2783aa524aaccbfda66831d59a054c31e/torch-2.5.1-cp311-cp311-manylinux2014_aarch64.whl", hash = "sha256:340ce0432cad0d37f5a31be666896e16788f1adf8ad7be481196b503dad675b9", size = 91919450 },
    { url = "https://files.pythonhosted.org/packages/0d/4a/e51420d46cfc90562e85af2fee912237c662ab31140ab179e49bd69401d6/torch-2.5.1-cp311-cp311-win_amd64.whl", hash = "sha256:603c52d2fe06433c18b747d25f5c333f9c1d58615620578c326d66f258686f9a", size = 203098237 },
    { url = "https://files.pythonhosted.org/packages/d0/db/5d9cbfbc7968d79c5c09a0bc0bc3735da079f2fd07cc10498a62b320a480/torch-2.5.1-cp311-none-macosx_11_0_arm64.whl", hash = "sha256:31f8c39660962f9ae4eeec995e3049b5492eb7360dd4f07377658ef4d728fa4c", size = 63884466 },
    { url = "https://files.pythonhosted.org/packages/8b/5c/36c114d120bfe10f9323ed35061bc5878cc74f3f594003854b0ea298942f/torch-2.5.1-cp312-cp312-manylinux1_x86_64.whl", hash = "sha256:ed231a4b3a5952177fafb661213d690a72caaad97d5824dd4fc17ab9e15cec03", size = 906389343 },
    { url = "https://files.pythonhosted.org/packages/6d/69/d8ada8b6e0a4257556d5b4ddeb4345ea8eeaaef3c98b60d1cca197c7ad8e/torch-2.5.1-cp312-cp312-manylinux2014_aarch64.whl", hash = "sha256:3f4b7f10a247e0dcd7ea97dc2d3bfbfc90302ed36d7f3952b0008d0df264e697", size = 91811673 },
    { url = "https://files.pythonhosted.org/packages/5f/ba/607d013b55b9fd805db2a5c2662ec7551f1910b4eef39653eeaba182c5b2/torch-2.5.1-cp312-cp312-win_amd64.whl", hash = "sha256:73e58e78f7d220917c5dbfad1a40e09df9929d3b95d25e57d9f8558f84c9a11c", size = 203046841 },
    { url = "https://files.pythonhosted.org/packages/57/6c/bf52ff061da33deb9f94f4121fde7ff3058812cb7d2036c97bc167793bd1/torch-2.5.1-cp312-none-macosx_11_0_arm64.whl", hash = "sha256:8c712df61101964eb11910a846514011f0b6f5920c55dbf567bff8a34163d5b1", size = 63858109 },
    { url = "https://files.pythonhosted.org/packages/69/72/20cb30f3b39a9face296491a86adb6ff8f1a47a897e4d14667e6cf89d5c3/torch-2.5.1-cp313-cp313-manylinux1_x86_64.whl", hash = "sha256:9b61edf3b4f6e3b0e0adda8b3960266b9009d02b37555971f4d1c8f7a05afed7", size = 906393265 },
]

[[package]]
name = "torch"
version = "2.5.1+cu124"
source = { registry = "https://download.pytorch.org/whl/cu124" }
resolution-markers = [
    "python_full_version < '3.12' and platform_machine == 'x86_64'",
    "python_full_version >= '3.12' and platform_machine == 'x86_64'",
]
dependencies = [
    { name = "filelock", marker = "platform_machine == 'x86_64'" },
    { name = "fsspec", marker = "platform_machine == 'x86_64'" },
    { name = "jinja2", marker = "platform_machine == 'x86_64'" },
    { name = "networkx", marker = "platform_machine == 'x86_64'" },
    { name = "nvidia-cublas-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-cupti-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-nvrtc-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-runtime-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cudnn-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cufft-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-curand-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusolver-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusparse-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nccl-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nvjitlink-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nvtx-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "setuptools", marker = "python_full_version >= '3.12' and platform_machine == 'x86_64'" },
    { name = "sympy", marker = "platform_machine == 'x86_64'" },
    { name = "triton", marker = "python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "typing-extensions", marker = "platform_machine == 'x86_64'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-linux_x86_64.whl", hash = "sha256:6b2966ede9affe2fd69e0765691ca723ec870e0c34c7761f4d5b8e318383fdaf" },
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-win_amd64.whl", hash = "sha256:6c8a7003ef1327479ede284b6e5ab3527d3900c2b2d401af15bcc50f2245a59f" },
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-linux_x86_64.whl", hash = "sha256:bf6484bfe5bc4f92a4a1a1bf553041505e19a911f717065330eb061afe0e14d7" },
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-win_amd64.whl", hash = "sha256:3c3f705fb125edbd77f9579fa11a138c56af8968a10fc95834cdd9fdf4f1f1a6" },
    { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp313-cp313-linux_x86_64.whl", hash = "sha256:e9bebf91ede89267577911da4b0709ac6113a0cff6a1c2202c046b1ec2a51601" },
]
```

You're getting the PyPI-based PyTorch on ARM, and the CUDA-enabled PyTorch on x86.

You could also write the requirements like this:

```toml
[project]
name = "minimal"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "torch==2.5.1 ; platform_machine != 'x86_64'",
    "torch==2.5.1+cu124 ; platform_machine == 'x86_64'",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cu124" }
]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

Then you'd get the CUDA-enabled PyTorch from the PyTorch index on x86, and the non-CUDA-enabled PyTorch from the PyTorch index on ARM.

---

_Comment by @bepuca on 2024-11-01 19:21_

Awesome! Thank you very much for taking the time, that does clarify. It was the missing piece to start migrating projects to `uv` 🎉 

---

_Comment by @charliermarsh on 2024-11-01 19:58_

Awesome, glad I could help!

---

_Closed by @charliermarsh on 2024-11-01 19:58_

---

_Referenced in [astral-sh/uv#6523](../../astral-sh/uv/pulls/6523.md) on 2024-11-01 23:51_

---

_Referenced in [astral-sh/uv#8930](../../astral-sh/uv/issues/8930.md) on 2024-11-08 10:27_

---

_Comment by @thistlillo on 2025-04-10 07:58_

I had the same problem on an Intel iMac without an nVidia card. Running uv to re-create the environment (originally built on an M1 MacBook), I get this error message:
```
$-> uv pip install -r requirements.txt 
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.6.0 has no wheels with a matching platform tag (e.g., `macosx_15_0_x86_64`) and you require torch==2.6.0, we can conclude that your requirements are unsatisfiable.

      hint: Wheels are available for `torch` (v2.6.0) on the following platforms: `manylinux_2_28_aarch64`, `manylinux1_x86_64`, `macosx_11_0_arm64`, `win_amd64`
```

But running this simple command (that installs torch 2.6.0), installation of PyTorch runs fine:
```
$-> uv pip install torch torchvision torchaudio
Resolved 13 packages in 267ms
Prepared 13 packages in 3.28s
Installed 13 packages in 279ms
...

```

After removing the `PyTorch` lines from `requirements.txt`, I was able to recreate the environment.

However, nothing works. I need to decode this error message:

```
A module that was compiled using NumPy 1.x cannot be run in
NumPy 2.2.4 as it may crash. To support both 1.x and 2.x
versions of NumPy, modules must be compiled with NumPy 2.0.
Some module may need to rebuild instead e.g. with 'pybind11>=2.12'.

If you are a user of the module, the easiest solution will be to
downgrade to 'numpy<2' or try to upgrade the affected module.
We expect that some modules will need time to support NumPy 2.
```

I have never been able to re-create an environment from the list of packages. Never, with any tool.


---

_Comment by @noopurphalak on 2025-08-27 04:02_

> So the `pyproject.toml` you have above looks right. If you inspect the lockfile:
> 
> [[package]]
> name = "torch"
> version = "2.5.1"
> source = { registry = "https://pypi.org/simple" }
> resolution-markers = [
>     "python_full_version < '3.12' and platform_machine != 'x86_64'",
>     "python_full_version >= '3.12' and platform_machine != 'x86_64'",
> ]
> dependencies = [
>     { name = "filelock", marker = "platform_machine != 'x86_64'" },
>     { name = "fsspec", marker = "platform_machine != 'x86_64'" },
>     { name = "jinja2", marker = "platform_machine != 'x86_64'" },
>     { name = "networkx", marker = "platform_machine != 'x86_64'" },
>     { name = "setuptools", marker = "python_full_version >= '3.12' and platform_machine != 'x86_64'" },
>     { name = "sympy", marker = "platform_machine != 'x86_64'" },
>     { name = "typing-extensions", marker = "platform_machine != 'x86_64'" },
> ]
> wheels = [
>     { url = "https://files.pythonhosted.org/packages/d1/35/e8b2daf02ce933e4518e6f5682c72fd0ed66c15910ea1fb4168f442b71c4/torch-2.5.1-cp311-cp311-manylinux1_x86_64.whl", hash = "sha256:de5b7d6740c4b636ef4db92be922f0edc425b65ed78c5076c43c42d362a45457", size = 906474467 },
>     { url = "https://files.pythonhosted.org/packages/40/04/bd91593a4ca178ece93ca55f27e2783aa524aaccbfda66831d59a054c31e/torch-2.5.1-cp311-cp311-manylinux2014_aarch64.whl", hash = "sha256:340ce0432cad0d37f5a31be666896e16788f1adf8ad7be481196b503dad675b9", size = 91919450 },
>     { url = "https://files.pythonhosted.org/packages/0d/4a/e51420d46cfc90562e85af2fee912237c662ab31140ab179e49bd69401d6/torch-2.5.1-cp311-cp311-win_amd64.whl", hash = "sha256:603c52d2fe06433c18b747d25f5c333f9c1d58615620578c326d66f258686f9a", size = 203098237 },
>     { url = "https://files.pythonhosted.org/packages/d0/db/5d9cbfbc7968d79c5c09a0bc0bc3735da079f2fd07cc10498a62b320a480/torch-2.5.1-cp311-none-macosx_11_0_arm64.whl", hash = "sha256:31f8c39660962f9ae4eeec995e3049b5492eb7360dd4f07377658ef4d728fa4c", size = 63884466 },
>     { url = "https://files.pythonhosted.org/packages/8b/5c/36c114d120bfe10f9323ed35061bc5878cc74f3f594003854b0ea298942f/torch-2.5.1-cp312-cp312-manylinux1_x86_64.whl", hash = "sha256:ed231a4b3a5952177fafb661213d690a72caaad97d5824dd4fc17ab9e15cec03", size = 906389343 },
>     { url = "https://files.pythonhosted.org/packages/6d/69/d8ada8b6e0a4257556d5b4ddeb4345ea8eeaaef3c98b60d1cca197c7ad8e/torch-2.5.1-cp312-cp312-manylinux2014_aarch64.whl", hash = "sha256:3f4b7f10a247e0dcd7ea97dc2d3bfbfc90302ed36d7f3952b0008d0df264e697", size = 91811673 },
>     { url = "https://files.pythonhosted.org/packages/5f/ba/607d013b55b9fd805db2a5c2662ec7551f1910b4eef39653eeaba182c5b2/torch-2.5.1-cp312-cp312-win_amd64.whl", hash = "sha256:73e58e78f7d220917c5dbfad1a40e09df9929d3b95d25e57d9f8558f84c9a11c", size = 203046841 },
>     { url = "https://files.pythonhosted.org/packages/57/6c/bf52ff061da33deb9f94f4121fde7ff3058812cb7d2036c97bc167793bd1/torch-2.5.1-cp312-none-macosx_11_0_arm64.whl", hash = "sha256:8c712df61101964eb11910a846514011f0b6f5920c55dbf567bff8a34163d5b1", size = 63858109 },
>     { url = "https://files.pythonhosted.org/packages/69/72/20cb30f3b39a9face296491a86adb6ff8f1a47a897e4d14667e6cf89d5c3/torch-2.5.1-cp313-cp313-manylinux1_x86_64.whl", hash = "sha256:9b61edf3b4f6e3b0e0adda8b3960266b9009d02b37555971f4d1c8f7a05afed7", size = 906393265 },
> ]
> 
> [[package]]
> name = "torch"
> version = "2.5.1+cu124"
> source = { registry = "https://download.pytorch.org/whl/cu124" }
> resolution-markers = [
>     "python_full_version < '3.12' and platform_machine == 'x86_64'",
>     "python_full_version >= '3.12' and platform_machine == 'x86_64'",
> ]
> dependencies = [
>     { name = "filelock", marker = "platform_machine == 'x86_64'" },
>     { name = "fsspec", marker = "platform_machine == 'x86_64'" },
>     { name = "jinja2", marker = "platform_machine == 'x86_64'" },
>     { name = "networkx", marker = "platform_machine == 'x86_64'" },
>     { name = "nvidia-cublas-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-cuda-cupti-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-cuda-nvrtc-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-cuda-runtime-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-cudnn-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-cufft-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-curand-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-cusolver-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-cusparse-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-nccl-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-nvjitlink-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "nvidia-nvtx-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "setuptools", marker = "python_full_version >= '3.12' and platform_machine == 'x86_64'" },
>     { name = "sympy", marker = "platform_machine == 'x86_64'" },
>     { name = "triton", marker = "python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'" },
>     { name = "typing-extensions", marker = "platform_machine == 'x86_64'" },
> ]
> wheels = [
>     { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-linux_x86_64.whl", hash = "sha256:6b2966ede9affe2fd69e0765691ca723ec870e0c34c7761f4d5b8e318383fdaf" },
>     { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-win_amd64.whl", hash = "sha256:6c8a7003ef1327479ede284b6e5ab3527d3900c2b2d401af15bcc50f2245a59f" },
>     { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-linux_x86_64.whl", hash = "sha256:bf6484bfe5bc4f92a4a1a1bf553041505e19a911f717065330eb061afe0e14d7" },
>     { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-win_amd64.whl", hash = "sha256:3c3f705fb125edbd77f9579fa11a138c56af8968a10fc95834cdd9fdf4f1f1a6" },
>     { url = "https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp313-cp313-linux_x86_64.whl", hash = "sha256:e9bebf91ede89267577911da4b0709ac6113a0cff6a1c2202c046b1ec2a51601" },
> ]
> You're getting the PyPI-based PyTorch on ARM, and the CUDA-enabled PyTorch on x86.
> 
> You could also write the requirements like this:
> 
> [project]
> name = "minimal"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> requires-python = ">=3.11"
> dependencies = [
>     "torch==2.5.1 ; platform_machine != 'x86_64'",
>     "torch==2.5.1+cu124 ; platform_machine == 'x86_64'",
> ]
> 
> [tool.uv.sources]
> torch = [
>     { index = "pytorch-cu124" }
> ]
> 
> [[tool.uv.index]]
> name = "pytorch-cu124"
> url = "https://download.pytorch.org/whl/cu124"
> explicit = true
> Then you'd get the CUDA-enabled PyTorch from the PyTorch index on x86, and the non-CUDA-enabled PyTorch from the PyTorch index on ARM.

I did all this, then if I do `uv pip freeze`, I don't see the `torch` package entry in it. Also, if I try to make a `pipeline` with `transformers`, I get the error `name 'torch' is not defined`. Can you help on this? I also tried doing `uv sync`, but it doesn't help either.

---
