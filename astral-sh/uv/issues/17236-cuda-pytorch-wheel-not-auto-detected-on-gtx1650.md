```yaml
number: 17236
title: CUDA PyTorch wheel not auto-detected on GTX1650 (CUDA 7.5)
type: issue
state: closed
author: aliphys
labels:
  - bug
assignees: []
created_at: 2025-12-24T23:44:13Z
updated_at: 2025-12-25T14:46:43Z
url: https://github.com/astral-sh/uv/issues/17236
synced_at: 2026-01-12T16:02:47Z
```

# CUDA PyTorch wheel not auto-detected on GTX1650 (CUDA 7.5)

---

_@aliphys_

### Summary

[Charlie's Twitter post](https://x.com/charliermarsh/status/1926250345887396217) states that _You can set `UV_TORCH_BACKEND=auto` and uv will automatically install the right CUDA-enabled PyTorch for your machine, zero configuration_.

Great! PyTorch wheel fatigue is a real thing, especially when you are developing for different systems/architectures and want to maintain a sense of ~portability~ sanity.

Following the [Using uv with PyTorch](https://docs.astral.sh/uv/guides/integration/pytorch/#automatic-backend-selection) documentation, I tried two ways to get the CUDA wheel to auto install:
1. **via pyproject.toml**
This is the contents of `pyproject.toml`, which was followed by `uv sync`.
```
[project]
name = "mypytorchenv"
version = "0.1.0"
description = "Experimental workspace for ML/DL projects"
requires-python = ">=3.9,<3.13"
dependencies = [
    "torch",
    "torchaudio",
    "torchvision",
]

[tool.uv.pip]
torch-backend = "auto"
```
2. **via terminal**
Ran command `uv pip install "torch" --torch-backend=auto`

For both cases, this is the output I get. Note that the PyTorch wheel for 2.9.1+cpu has been installed. 
```
Python 3.12.10 (tags/v3.12.10:0cc8128, Apr  8 2025, 12:21:36) [MSC v.1943 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
Ctrl click to launch VS Code Native REPL
>>> import torch
>>> print(torch.__version__)
2.9.1+cpu
```

I would ideally expect that the latest wheel with support for CUDA 7.5 is instead installed, given that this is the [most recent CUDA version that the GTX 1650 supports](https://developer.nvidia.com/cuda/gpus).

<img width="1102" height="729" alt="Image" src="https://github.com/user-attachments/assets/e845d796-0f7f-420b-8d2c-2c2698e90e3d" />

Could it be that since there are [no official CUDA 7.5 windows binary packages for PyTorch](https://discuss.pytorch.org/t/where-to-get-a-pytorch-version-with-cuda-7-5/136446/3?u=aliphys), you have decided to fall back to the cpu-only version instead? In which case, it would be great to have a warning telling the user `CUDA 7.5 device detected. No official PyTorch builds available, falling back to cpu build.`

### Platform

Windows 11 64 bit

### Version

uv 0.9.18 (0cee76417 2025-12-16)

### Python version

Python 3.12.10

---

_Label `bug` added by @aliphys on 2025-12-24 23:44_

---

_Comment by @charliermarsh on 2025-12-25 03:01_

Can you share the output with the `--verbose` flag?

Note that 7.5 is not a CUDA version -- it's a compute capability. You can use a significantly newer CUDA runtime version with that GPU, likely up to and including CUDA 13.1.

---

_Closed by @aliphys on 2025-12-25 14:22_

---

_Comment by @aliphys on 2025-12-25 14:35_

**(After the email correspondence, was able to attach response by moving --verbose log to gist. Attaching here for future reference)**

---

Hello @charliermarsh 👋. Thanks for the heads up regarding the CUDA / compute capability distinction. 

## CUDA wheel installation attempt with --verbose

I ran the commands in a new directory, without any `.venv` or `uv lock` in the folder. This is the result of their verbose output.

<details><summary>via pyproject.toml</summary>


https://gist.github.com/aliphys/6201ebdbe847aa6504ea4e167c87175a


</details>

<details><summary>via terminal</summary>

https://gist.github.com/aliphys/a3667e847758710e8284553551858273

</details>

---

## PyTorch Check

<details><summary>via pyproject.toml</summary>

```
(uvtestdir) PS C:\Users\user\Desktop\uvtestdir> python
Python 3.12.10 (tags/v3.12.10:0cc8128, Apr  8 2025, 12:21:36) [MSC v.1943 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> print(torch.__version__)
2.9.1+cpu
```

</details>

<details><summary>via terminal</summary>

```
(uvtestdir) PS C:\Users\user\Desktop\uvtestdir> python
Python 3.13.5 (main, Jun 12 2025, 12:42:35) [MSC v.1943 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
C:\Users\user\Desktop\uvtestdir\.venv\Lib\site-packages\torch\_subclasses\functional_tensor.py:279: UserWarning: Failed to initialize NumPy: No module named 'numpy' (Triggered internally at C:\actions-runner\_work\pytorch\pytorch\pytorch\torch\csrc\utils\tensor_numpy.cpp:84.)
  cpu = _conversion_method_template(device=torch.device("cpu"))
>>> print(torch.__version__)
2.9.1+cu130
```

</details>

## Summary

When running in a clean, new directory with NO `uv.lock` or `.venv` file:

|Item|`pyproject.toml`|`uv pip install "torch" --torch-backend=auto`|details|
|-|-|-|-|
|PyTorch wheel |2.9.1+cpu|2.9.1+cu130| The exact same version of PyTorch (2.9.1) is installed. However, the key difference here is that the terminal installs the CUDA version. Whereas the .toml approach does falls back to the cpu wheel. This is in spite of the fact that the verbose output does make an attempt to install a CUDA wheel, so it seems that the need for CUDA is recognised, but along the way our friend gets lost. 😄|
|Numpy |2.4.0| N/A|Numpy is not specified to be installed manually, neither in the `.toml` file nor manually. It is expected that this is installed, with uv taking care of the version conflicts. The `.toml` approach does resolve this issue. **(Note, just realised this could be because I specified torchvision to be installed in the `.toml` and not in the terminal)** |
|Python | 3.12.10|3.13.5|OK!|
|sys_platform variable| "linux"| N/A| It seems that the `sys_platform` is identifying my Windows 11 system as a Linux system. I am NOT running uv under WSL. This additional variable, could be the reason why the CUDA wheels are not installed?|
|Detected cuBlas wheel|nvidia_cublas_cu12-12.8.4.1-py3-none-manylinux_2_27_aarch64.whl|N/A|uv detects a linux wheel for cublas. I didn't see a message about this for the terminal approach, so I could not compare.|

---

_Comment by @aliphys on 2025-12-25 14:41_

@charliermarsh 's suggestion over email:
```
Thanks for replying! What might be happening here: the `--torch-backend` flags are only supported on the `uv pip` commands right now. We haven't added support to `uv sync` yet. Can you try `uv venv` then `uv pip install --torch-backend auto ...`?
```

✅ Based on this, I now have an interim solution which works well enough. 

```
uv venv --python '>=3.9, <3.13'; uv pip install -r requirements.txt --torch-backend=auto --verbose
```

- Terminal verbose output: https://gist.github.com/aliphys/4a946eeb391353abe9eb1ae4e5555f5a

PyTorch wheel `2.9.1+cu130` together with Numpy `2.4.0` are both happily installed 😃
```
PS C:\Users\user\Desktop\uvtestdir> .\.venv\Scripts\activate
(uvtestdir) PS C:\Users\user\Desktop\uvtestdir> python
Python 3.12.12 (main, Dec  9 2025, 19:02:55) [MSC v.1944 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> print(torch.__version__)
2.9.1+cu130
>>> import numpy
>>> print(numpy.__version__)
2.4.0
```

As a user, I would prefer to use `pyproject.toml` instead of a `requirements.txt` when we have PyTorch CUDA wheels. Looking forward to seeing this implemented in the future 🤞

---
