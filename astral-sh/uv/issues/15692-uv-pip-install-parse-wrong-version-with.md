```yaml
number: 15692
title: "uv pip install parse wrong version with \"+\""
type: issue
state: closed
author: cos120
labels:
  - bug
assignees: []
created_at: 2025-09-05T04:16:44Z
updated_at: 2025-09-06T02:36:16Z
url: https://github.com/astral-sh/uv/issues/15692
synced_at: 2026-01-12T16:02:15Z
```

# uv pip install parse wrong version with "+"

---

_@cos120_

### Summary

Hi, I am using uv create env with system
```bash
uv venv --relocatable --no-managed-python --system-site-packages test
```
And I have installed `torch==2.8.0+cu128` in my system site, when I use
`uv pip install accelerate -v --dry-run`, I found uv ignore the version `torch==2.8.0+cu128`
```bash
DEBUG Adding transitive dependency for accelerate==1.10.1: torch>=2.0.0
DEBUG Found fresh response for: https://mirrors.aliyun.com/pypi/simple/torch/
DEBUG Found fresh response for: https://mirrors.aliyun.com/pypi/packages/99/a8/6acf48d48838fb8fe480597d98a0668c2beb02ee4755cc136de92a0a956f/torch-2.8.0-cp312-cp312-manylinux_2_28_x86_64.whl
DEBUG Searching for a compatible version of torch (>=2.0.0)
DEBUG Selecting: torch==2.8.0 [compatible] (torch-2.8.0-cp312-cp312-manylinux_2_28_x86_64.whl)
DEBUG Adding transitive dependency for torch==2.8.0: filelock*
DEBUG Adding transitive dependency for torch==2.8.0: typing-extensions>=4.10.0
DEBUG Adding transitive dependency for torch==2.8.0: setuptools{python_full_version >= '3.12'}*
DEBUG Adding transitive dependency for torch==2.8.0: sympy>=1.13.3
DEBUG Adding transitive dependency for torch==2.8.0: networkx*
DEBUG Adding transitive dependency for torch==2.8.0: jinja2*
DEBUG Adding transitive dependency for torch==2.8.0: fsspec*
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cuda-nvrtc-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.8.93, <12.8.93+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cuda-runtime-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.8.90, <12.8.90+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cuda-cupti-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.8.90, <12.8.90+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cudnn-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=9.10.2.21, <9.10.2.21+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cublas-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.8.4.1, <12.8.4.1+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cufft-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=11.3.3.83, <11.3.3.83+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-curand-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=10.3.9.90, <10.3.9.90+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cusolver-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=11.7.3.90, <11.7.3.90+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cusparse-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.5.8.93, <12.5.8.93+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cusparselt-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=0.7.1, <0.7.1+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-nccl-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=2.27.3, <2.27.3+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-nvtx-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.8.90, <12.8.90+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-nvjitlink-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=12.8.93, <12.8.93+
DEBUG Adding transitive dependency for torch==2.8.0: nvidia-cufile-cu12{platform_machine == 'x86_64' and sys_platform == 'linux'}>=1.13.1.3, <1.13.1.3+
DEBUG Adding transitive dependency for torch==2.8.0: triton{platform_machine == 'x86_64' and sys_platform == 'linux'}>=3.4.0, <3.4.0+
```
But I use pip in venv native, 
```bash
pip install accelerate --break-system-packages --dry-run

Collecting accelerate
  Using cached https://mirrors.aliyun.com/pypi/packages/5f/a0/d9ef19f780f319c21ee90ecfef4431cbeeca95bec7f14071785c17b6029b/accelerate-1.10.1-py3-none-any.whl (374 kB)
Requirement already satisfied: numpy<3.0.0,>=1.17 in /usr/local/lib/python3.12/dist-packages (from accelerate) (1.26.4)
Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.12/dist-packages (from accelerate) (25.0)
Requirement already satisfied: psutil in /usr/local/lib/python3.12/dist-packages (from accelerate) (7.0.0)
Requirement already satisfied: pyyaml in /usr/local/lib/python3.12/dist-packages (from accelerate) (6.0.2)
Requirement already satisfied: torch>=2.0.0 in /usr/local/lib/python3.12/dist-packages (from accelerate) (2.8.0+cu128)
Requirement already satisfied: huggingface_hub>=0.21.0 in /usr/local/lib/python3.12/dist-packages (from accelerate) (0.34.4)
Requirement already satisfied: safetensors>=0.4.3 in /usr/local/lib/python3.12/dist-packages (from accelerate) (0.6.2)
Requirement already satisfied: filelock in /usr/local/lib/python3.12/dist-packages (from huggingface_hub>=0.21.0->accelerate) (3.19.1)
Requirement already satisfied: fsspec>=2023.5.0 in /usr/local/lib/python3.12/dist-packages (from huggingface_hub>=0.21.0->accelerate) (2024.6.1)
Requirement already satisfied: requests in /usr/local/lib/python3.12/dist-packages (from huggingface_hub>=0.21.0->accelerate) (2.32.5)
Requirement already satisfied: tqdm>=4.42.1 in /usr/local/lib/python3.12/dist-packages (from huggingface_hub>=0.21.0->accelerate) (4.67.1)
Requirement already satisfied: typing-extensions>=3.7.4.3 in /usr/local/lib/python3.12/dist-packages (from huggingface_hub>=0.21.0->accelerate) (4.12.2)
Requirement already satisfied: hf-xet<2.0.0,>=1.1.3 in /usr/local/lib/python3.12/dist-packages (from huggingface_hub>=0.21.0->accelerate) (1.1.9)
Requirement already satisfied: setuptools in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (79.0.1)
Requirement already satisfied: sympy>=1.13.3 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (1.13.3)
Requirement already satisfied: networkx in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (3.3)
Requirement already satisfied: jinja2 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (3.1.6)
Requirement already satisfied: nvidia-cuda-nvrtc-cu12==12.8.93 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (12.8.93)
Requirement already satisfied: nvidia-cuda-runtime-cu12==12.8.90 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (12.8.90)
Requirement already satisfied: nvidia-cuda-cupti-cu12==12.8.90 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (12.8.90)
Requirement already satisfied: nvidia-cudnn-cu12==9.10.2.21 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (9.10.2.21)
Requirement already satisfied: nvidia-cublas-cu12==12.8.4.1 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (12.8.4.1)
Requirement already satisfied: nvidia-cufft-cu12==11.3.3.83 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (11.3.3.83)
Requirement already satisfied: nvidia-curand-cu12==10.3.9.90 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (10.3.9.90)
Requirement already satisfied: nvidia-cusolver-cu12==11.7.3.90 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (11.7.3.90)
Requirement already satisfied: nvidia-cusparse-cu12==12.5.8.93 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (12.5.8.93)
Requirement already satisfied: nvidia-cusparselt-cu12==0.7.1 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (0.7.1)
Requirement already satisfied: nvidia-nccl-cu12==2.27.3 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (2.27.3)
Requirement already satisfied: nvidia-nvtx-cu12==12.8.90 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (12.8.90)
Requirement already satisfied: nvidia-nvjitlink-cu12==12.8.93 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (12.8.93)
Requirement already satisfied: nvidia-cufile-cu12==1.13.1.3 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (1.13.1.3)
Requirement already satisfied: triton==3.4.0 in /usr/local/lib/python3.12/dist-packages (from torch>=2.0.0->accelerate) (3.4.0)
Requirement already satisfied: mpmath<1.4,>=1.1.0 in /usr/local/lib/python3.12/dist-packages (from sympy>=1.13.3->torch>=2.0.0->accelerate) (1.3.0)
Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib/python3.12/dist-packages (from jinja2->torch>=2.0.0->accelerate) (2.1.5)
Requirement already satisfied: charset_normalizer<4,>=2 in /usr/local/lib/python3.12/dist-packages (from requests->huggingface_hub>=0.21.0->accelerate) (3.4.3)
Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.12/dist-packages (from requests->huggingface_hub>=0.21.0->accelerate) (3.10)
Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.12/dist-packages (from requests->huggingface_hub>=0.21.0->accelerate) (2.5.0)
Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.12/dist-packages (from requests->huggingface_hub>=0.21.0->accelerate) (2025.8.3)
Would install accelerate-1.10.1
```

Looks like `uv pip install do not parse version with '+'`?

### Platform

Ubuntu24.04 amd64

### Version

uv 0.8.15

### Python version

3.12.3

---

_Label `bug` added by @cos120 on 2025-09-05 04:16_

---

_Comment by @konstin on 2025-09-05 09:29_

It's a different problem: uv doesn't see packages in the system site packages, this is a duplicate of #2500. If you want to work around this, you can use a hack by setting an override with `torch==2.8.0+cu128; python_version < "0"` which makes uv pretend it doesn't need to install torch. This is something I usually wouldn't recommend, I rather recommend installing torch into the venv too, but in some cases it may not be avoidable.

---

_Comment by @cos120 on 2025-09-05 10:52_

Thanks for your clearly! I am using uv for building multiple user env based on one readonly system env in my docker image. So if I want to separate multiple env from system, what is the best practice? @konstin 


---

_Comment by @konstin on 2025-09-05 11:21_

I would create a venv in the docker image and install both torch and the other packages in that venv.

---

_Comment by @notatallshaw on 2025-09-05 15:41_

Some further reading you may be interested in: https://hynek.me/articles/docker-virtualenv/

There is a general misconception that using a docker image replaces the need to use a virtual environment (venv), but unless the docker image has been very carefully constructed this is often not true, and using a venv is often still useful to produces more consistent  images across multiple builds.

---

_Closed by @charliermarsh on 2025-09-06 02:36_

---
