```yaml
number: 16532
title: How can you install xformers with uv ?
type: issue
state: closed
author: muhammad-fiaz
labels:
  - question
assignees: []
created_at: 2025-10-31T07:18:25Z
updated_at: 2025-10-31T09:51:45Z
url: https://github.com/astral-sh/uv/issues/16532
synced_at: 2026-01-10T03:23:55Z
```

# How can you install xformers with uv ?

---

_Issue opened by @muhammad-fiaz on 2025-10-31 07:18_

### Question

i have been getting error

```bash
PS C:\Users\user\Downloads> uv add xformers --default-index https://download.pytorch.org/whl/cu129
  × No solution found when resolving dependencies for split (markers: (python_full_version >=
  │ '3.15' and platform_machine == 'aarch64' and platform_python_implementation == 'CPython' and       
  │ sys_platform == 'linux') or (python_full_version >= '3.14' and platform_machine != 'aarch64' and   
  │ sys_platform == 'linux') or (python_full_version >= '3.14' and platform_python_implementation      
  │ != 'CPython' and sys_platform == 'linux') or (python_full_version >= '3.14' and sys_platform       
  │ == 'win32')):
  ╰─▶ Because gradio was not found in the package registry and your project depends on gradio>=5.12,   
      we can conclude that your project's requirements are unsatisfiable.
      And because your project requires charisma[dev], we can conclude that your project's
      requirements are unsatisfiable.

      hint: While the active Python version is 3.12, the resolution failed for other Python versions   
      supported by your project. Consider limiting your project's supported Python versions using      
      `requires-python`.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen`     
        flag to skip locking and syncing.

PS C:\Users\user\Downloads> uv sync                                                
Resolved 178 packages in 4.02s                                                                         
error: Distribution `xformers==0.0.33+5d4b92a5.d20251029 @ registry+https://download.pytorch.org/whl/cu128` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Windows (`win_amd64`), but `xformers` (v0.0.33+5d4b92a5.d20251029) only has wheels for the following platform: `linux_x86_64`; consider adding "sys_platform == 'win32' and platform_machine == 'AMD64'" to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
 PS C:\Users\user\Downloads> error: Distribution `xformers==0.0.33+5d4b92a5.d20251029 @ registry+https://download.pytorch.org/whl/cu128` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Windows (`win_amd64`), but `xformers` (v0.0.33+5d4b92a5.d20251029) only has wheels for the following platform: `linux_x86_64`; consider adding "sys_platform == 'win32' and platform_machine == 'AMD64'" to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels

```

i tried to installed with `uv` 

```toml
dependencies = [
  "torch>=2.5.0",
  "torchvision>=0.20.0",
  "logly>=0.1.6",
  "requests>=2.32",
  "regex>=2024.11",
  "safetensors>=0.5",
  "gradio>=5.12",
  "gradio-client>=1.5.4",
  "typer>=0.9.0",
  "toml>=0.10.2",
  "notion-client>=2.2.1",
  "rich>=14.2.0",
  "unsloth",
  "xformers>=0.0.20",
]
[tool.uv.sources]
unsloth = { git = "https://github.com/unslothai/unsloth.git" }
xformers = [
  { index = "pytorch-cu128", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true

```

still does not work  and is there any good example use cases for this to install without using `optional-dependencies` , this issue is related to `Feature Request` #16522  and when i tried this with `pip` it works fine

below is `pip` installation in my `.venv`
```bash
 PS C:\Users\user\Downloads> pip3 install -U xformers --index-url https://download.pytorch.org/whl/cu129
Defaulting to user installation because normal site-packages is not writeable
Looking in indexes: https://download.pytorch.org/whl/cu129, https://pypi.ngc.nvidia.com
Collecting xformers
  Downloading https://download.pytorch.org/whl/cu129/xformers-0.0.32.post2-cp39-abi3-win_amd64.whl.metadata (1.1 kB)
Requirement already satisfied: numpy in c:\users\user\appdata\roaming\python\python312\site-packages (from xformers) (1.26.4)
Requirement already satisfied: torch==2.8.0 in c:\users\user\appdata\roaming\python\python312\site-packages (from xformers) (2.8.0+cu129)
Requirement already satisfied: filelock in c:\users\user\appdata\roaming\python\python312\site-packages (from torch==2.8.0->xformers) (3.19.1)
Requirement already satisfied: typing-extensions>=4.10.0 in c:\users\user\appdata\roaming\python\python312\site-packages (from torch==2.8.0->xformers) (4.15.0)
Requirement already satisfied: sympy>=1.13.3 in c:\users\user\appdata\roaming\python\python312\site-packages (from torch==2.8.0->xformers) (1.14.0)
Requirement already satisfied: networkx in c:\users\user\appdata\roaming\python\python312\site-packages (from torch==2.8.0->xformers) (3.5)
Requirement already satisfied: jinja2 in c:\users\user\appdata\roaming\python\python312\site-packages (from torch==2.8.0->xformers) (3.1.6)
Requirement already satisfied: fsspec in c:\users\user\appdata\roaming\python\python312\site-packages (from torch==2.8.0->xformers) (2025.3.0)
Requirement already satisfied: setuptools in c:\users\user\appdata\roaming\python\python312\site-packages (from torch==2.8.0->xformers) (80.9.0)
Requirement already satisfied: mpmath<1.4,>=1.1.0 in c:\users\user\appdata\roaming\python\python312\site-packages (from sympy>=1.13.3->torch==2.8.0->xformers) (1.3.0)
Requirement already satisfied: MarkupSafe>=2.0 in c:\users\user\appdata\roaming\python\python312\site-packages (from jinja2->torch==2.8.0->xformers) (3.0.3)
Downloading https://download.pytorch.org/whl/cu129/xformers-0.0.32.post2-cp39-abi3-win_amd64.whl (106.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 106.0/106.0 MB 3.5 MB/s  0:00:30
Installing collected packages: xformers
Successfully installed xformers-0.0.32.post2
```

### Platform

Windows 11

### Version

v0.9.7

---

_Label `question` added by @muhammad-fiaz on 2025-10-31 07:18_

---

_Comment by @muhammad-fiaz on 2025-10-31 09:51_

finally, I was able to resolve the issue by configuring **uv** to pull `torch` and `xformers` from the **correct CUDA-specific PyTorch index**, and by adding `extra-build-dependencies` by `uv` so that `xformers` can compile properly on Windows.

#### **Working `pyproject.toml` setup:**

```toml
[project]
dependencies = [
  "torch>=2.5.0",
  "torchvision>=0.20.0",
  "torchaudio>=2.5.0",
  "xformers>=0.0.20",
  # ...other dependencies
]

[tool.uv.extra-build-dependencies]
xformers = ["torch", "ninja"]

[[tool.uv.index]]
name = "pytorch-cu129"
url = "https://download.pytorch.org/whl/cu129"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch-cu129" }
torchvision = { index = "pytorch-cu129" }
torchaudio = { index = "pytorch-cu129" }
xformers = { index = "pytorch-cu129" }
```


if you want full working configuration you can look at my [pyproject.toml](https://github.com/muhammad-fiaz/Charisma/blob/main/pyproject.toml)



---

_Closed by @muhammad-fiaz on 2025-10-31 09:51_

---
