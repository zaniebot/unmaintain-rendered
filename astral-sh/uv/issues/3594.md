```yaml
number: 3594
title: "can't install flash attention pre-compiled wheels because of a version mismatch"
type: issue
state: closed
author: isidentical
labels:
  - compatibility
assignees: []
created_at: 2024-05-14T21:49:28Z
updated_at: 2024-05-15T03:02:18Z
url: https://github.com/astral-sh/uv/issues/3594
synced_at: 2026-01-10T05:31:37Z
```

# can't install flash attention pre-compiled wheels because of a version mismatch

---

_Issue opened by @isidentical on 2024-05-14 21:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
The following command works with `pip` directly, but not with `uv pip`. There are a couple other flash attention issues in the tracker but all of them seem to be building it, not installing a pre-built version. Any idea what might be causing this?

```
$ uv pip install --verbose https://github.com/Dao-AILab/flash-attention/releases/download/v2.5.8/
flash_attn-2.5.8+cu122torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.wh
```

version=uv 0.1.44

<details>
  <summary>Verbose logs</summary>
  
  ```
INFO Found a virtualenv through VIRTUAL_ENV at: /home/user/.venv
DEBUG Cached interpreter info for Python 3.10.12, skipping probing: .venv/bin/python
DEBUG Using Python 3.10.12 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
DEBUG At least one requirement is not satisfied: https://github.com/Dao-AILab/flash-attention/releases/download/v2.5.8/flash_attn-2.5.8+cu122torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl
DEBUG Using registry request timeout of 30s
DEBUG Found fresh response for: https://github.com/Dao-AILab/flash-attention/releases/download/v2.5.8/flash_attn-2.5.8+cu122torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl
DEBUG Solving with target Python version 3.10.12
DEBUG Adding direct dependency: flash-attn*
DEBUG Searching for a compatible version of flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.5.8/flash_attn-2.5.8+cu122torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl (*)
DEBUG Adding transitive dependency for flash-attn==2.5.8: torch*
DEBUG Adding transitive dependency for flash-attn==2.5.8: einops*
DEBUG Adding transitive dependency for flash-attn==2.5.8: packaging*
DEBUG Adding transitive dependency for flash-attn==2.5.8: ninja*
DEBUG Found fresh response for: https://pypi.org/simple/torch/
DEBUG Found fresh response for: https://pypi.org/simple/einops/
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found installed version of torch==2.3.0 that satisfies preference in *
DEBUG Found installed version of einops==0.8.0 that satisfies preference in *
DEBUG Found fresh response for: https://pypi.org/simple/ninja/
DEBUG Found installed version of packaging==24.0 that satisfies preference in *
DEBUG Found installed version of ninja==1.11.1.1 that satisfies preference in *
DEBUG Searching for a compatible version of torch (*)
DEBUG Found installed version of torch==2.3.0 that satisfies preference in *
DEBUG Selecting: torch==2.3.0 (installed)
DEBUG Adding transitive dependency for torch==2.3.0: filelock*
DEBUG Adding transitive dependency for torch==2.3.0: typing-extensions>=4.8.0
DEBUG Adding transitive dependency for torch==2.3.0: sympy*
DEBUG Adding transitive dependency for torch==2.3.0: networkx*
DEBUG Adding transitive dependency for torch==2.3.0: jinja2*
DEBUG Adding transitive dependency for torch==2.3.0: fsspec*
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-cuda-nvrtc-cu12==12.1.105
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-cuda-runtime-cu12==12.1.105
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-cuda-cupti-cu12==12.1.105
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-cudnn-cu12==8.9.2.26
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-cublas-cu12==12.1.3.1
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-cufft-cu12==11.0.2.54
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-curand-cu12==10.3.2.106
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-cusolver-cu12==11.4.5.107
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-cusparse-cu12==12.1.0.106
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-nccl-cu12==2.20.5
DEBUG Adding transitive dependency for torch==2.3.0: nvidia-nvtx-cu12==12.1.105
DEBUG Adding transitive dependency for torch==2.3.0: triton==2.3.0
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://pypi.org/simple/filelock/
DEBUG Found fresh response for: https://pypi.org/simple/sympy/
DEBUG Found fresh response for: https://pypi.org/simple/networkx/
DEBUG Found fresh response for: https://pypi.org/simple/jinja2/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-nvrtc-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/fsspec/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-runtime-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cudnn-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cublas-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cufft-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cusolver-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cusparse-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-nccl-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-nvtx-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-cupti-cu12/
DEBUG Found installed version of typing-extensions==4.11.0 that satisfies preference in >=4.8.0
DEBUG Found fresh response for: https://pypi.org/simple/triton/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-curand-cu12/
DEBUG Found installed version of filelock==3.14.0 that satisfies preference in *
DEBUG Found installed version of sympy==1.12 that satisfies preference in *
DEBUG Found installed version of networkx==3.3 that satisfies preference in *
DEBUG Found installed version of jinja2==3.1.4 that satisfies preference in *
DEBUG Found installed version of nvidia-cuda-nvrtc-cu12==12.1.105 that satisfies preference in ==12.1.105
DEBUG Found installed version of fsspec==2024.3.1 that satisfies preference in *
DEBUG Found installed version of nvidia-cuda-runtime-cu12==12.1.105 that satisfies preference in ==12.1.105
DEBUG Found installed version of nvidia-cudnn-cu12==8.9.2.26 that satisfies preference in ==8.9.2.26
DEBUG Found installed version of nvidia-cublas-cu12==12.1.3.1 that satisfies preference in ==12.1.3.1
DEBUG Found installed version of nvidia-cufft-cu12==11.0.2.54 that satisfies preference in ==11.0.2.54
DEBUG Found installed version of nvidia-cusolver-cu12==11.4.5.107 that satisfies preference in ==11.4.5.107
DEBUG Found installed version of nvidia-cusparse-cu12==12.1.0.106 that satisfies preference in ==12.1.0.106
DEBUG Found installed version of nvidia-nccl-cu12==2.20.5 that satisfies preference in ==2.20.5
DEBUG Found installed version of nvidia-nvtx-cu12==12.1.105 that satisfies preference in ==12.1.105
DEBUG Found installed version of nvidia-cuda-cupti-cu12==12.1.105 that satisfies preference in ==12.1.105
DEBUG Found installed version of triton==2.3.0 that satisfies preference in ==2.3.0
DEBUG Found installed version of nvidia-curand-cu12==10.3.2.106 that satisfies preference in ==10.3.2.106
DEBUG Searching for a compatible version of nvidia-cuda-nvrtc-cu12 (==12.1.105)
DEBUG Found installed version of nvidia-cuda-nvrtc-cu12==12.1.105 that satisfies preference in ==12.1.105
DEBUG Selecting: nvidia-cuda-nvrtc-cu12==12.1.105 (installed)
DEBUG Searching for a compatible version of nvidia-cuda-runtime-cu12 (==12.1.105)
DEBUG Found installed version of nvidia-cuda-runtime-cu12==12.1.105 that satisfies preference in ==12.1.105
DEBUG Selecting: nvidia-cuda-runtime-cu12==12.1.105 (installed)
DEBUG Searching for a compatible version of nvidia-cuda-cupti-cu12 (==12.1.105)
DEBUG Found installed version of nvidia-cuda-cupti-cu12==12.1.105 that satisfies preference in ==12.1.105
DEBUG Selecting: nvidia-cuda-cupti-cu12==12.1.105 (installed)
DEBUG Searching for a compatible version of nvidia-cudnn-cu12 (==8.9.2.26)
DEBUG Found installed version of nvidia-cudnn-cu12==8.9.2.26 that satisfies preference in ==8.9.2.26
DEBUG Selecting: nvidia-cudnn-cu12==8.9.2.26 (installed)
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==8.9.2.26: nvidia-cublas-cu12*
DEBUG Searching for a compatible version of nvidia-cublas-cu12 (==12.1.3.1)
DEBUG Found installed version of nvidia-cublas-cu12==12.1.3.1 that satisfies preference in ==12.1.3.1
DEBUG Selecting: nvidia-cublas-cu12==12.1.3.1 (installed)
DEBUG Searching for a compatible version of nvidia-cufft-cu12 (==11.0.2.54)
DEBUG Found installed version of nvidia-cufft-cu12==11.0.2.54 that satisfies preference in ==11.0.2.54
DEBUG Selecting: nvidia-cufft-cu12==11.0.2.54 (installed)
DEBUG Searching for a compatible version of nvidia-curand-cu12 (==10.3.2.106)
DEBUG Found installed version of nvidia-curand-cu12==10.3.2.106 that satisfies preference in ==10.3.2.106
DEBUG Selecting: nvidia-curand-cu12==10.3.2.106 (installed)
DEBUG Searching for a compatible version of nvidia-cusolver-cu12 (==11.4.5.107)
DEBUG Found installed version of nvidia-cusolver-cu12==11.4.5.107 that satisfies preference in ==11.4.5.107
DEBUG Selecting: nvidia-cusolver-cu12==11.4.5.107 (installed)
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cublas-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-nvjitlink-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.4.5.107: nvidia-cusparse-cu12*
DEBUG Searching for a compatible version of nvidia-cusparse-cu12 (==12.1.0.106)
DEBUG Found installed version of nvidia-cusparse-cu12==12.1.0.106 that satisfies preference in ==12.1.0.106
DEBUG Selecting: nvidia-cusparse-cu12==12.1.0.106 (installed)
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.1.0.106: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-nccl-cu12 (==2.20.5)
DEBUG Found installed version of nvidia-nccl-cu12==2.20.5 that satisfies preference in ==2.20.5
DEBUG Selecting: nvidia-nccl-cu12==2.20.5 (installed)
DEBUG Searching for a compatible version of nvidia-nvtx-cu12 (==12.1.105)
DEBUG Found installed version of nvidia-nvtx-cu12==12.1.105 that satisfies preference in ==12.1.105
DEBUG Selecting: nvidia-nvtx-cu12==12.1.105 (installed)
DEBUG Searching for a compatible version of triton (==2.3.0)
DEBUG Found installed version of triton==2.3.0 that satisfies preference in ==2.3.0
DEBUG Selecting: triton==2.3.0 (installed)
DEBUG Adding transitive dependency for triton==2.3.0: filelock*
DEBUG Searching for a compatible version of einops (*)
DEBUG Found installed version of einops==0.8.0 that satisfies preference in *
DEBUG Selecting: einops==0.8.0 (installed)
DEBUG Searching for a compatible version of packaging (*)
DEBUG Found installed version of packaging==24.0 that satisfies preference in *
DEBUG Selecting: packaging==24.0 (installed)
DEBUG Searching for a compatible version of ninja (*)
DEBUG Found installed version of ninja==1.11.1.1 that satisfies preference in *
DEBUG Selecting: ninja==1.11.1.1 (installed)
DEBUG Searching for a compatible version of filelock (*)
DEBUG Found installed version of filelock==3.14.0 that satisfies preference in *
DEBUG Selecting: filelock==3.14.0 (installed)
DEBUG Searching for a compatible version of typing-extensions (>=4.8.0)
DEBUG Found installed version of typing-extensions==4.11.0 that satisfies preference in >=4.8.0
DEBUG Selecting: typing-extensions==4.11.0 (installed)
DEBUG Searching for a compatible version of sympy (*)
DEBUG Found installed version of sympy==1.12 that satisfies preference in *
DEBUG Selecting: sympy==1.12 (installed)
DEBUG Adding transitive dependency for sympy==1.12: mpmath>=0.19
DEBUG Searching for a compatible version of networkx (*)
DEBUG Found installed version of networkx==3.3 that satisfies preference in *
DEBUG Selecting: networkx==3.3 (installed)
DEBUG Searching for a compatible version of jinja2 (*)
DEBUG Found installed version of jinja2==3.1.4 that satisfies preference in *
DEBUG Selecting: jinja2==3.1.4 (installed)
DEBUG Adding transitive dependency for jinja2==3.1.4: markupsafe>=2.0
DEBUG Searching for a compatible version of fsspec (*)
DEBUG Found installed version of fsspec==2024.3.1 that satisfies preference in *
DEBUG Selecting: fsspec==2024.3.1 (installed)
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-nvjitlink-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/mpmath/
DEBUG Found installed version of nvidia-nvjitlink-cu12==12.4.127 that satisfies preference in *
DEBUG Found installed version of mpmath==1.3.0 that satisfies preference in >=0.19
DEBUG Searching for a compatible version of nvidia-nvjitlink-cu12 (*)
DEBUG Found installed version of nvidia-nvjitlink-cu12==12.4.127 that satisfies preference in *
DEBUG Selecting: nvidia-nvjitlink-cu12==12.4.127 (installed)
DEBUG Searching for a compatible version of mpmath (>=0.19)
DEBUG Found installed version of mpmath==1.3.0 that satisfies preference in >=0.19
DEBUG Selecting: mpmath==1.3.0 (installed)
DEBUG Found fresh response for: https://pypi.org/simple/markupsafe/
DEBUG Found installed version of markupsafe==2.1.5 that satisfies preference in >=2.0
DEBUG Searching for a compatible version of markupsafe (>=2.0)
DEBUG Found installed version of markupsafe==2.1.5 that satisfies preference in >=2.0
DEBUG Selecting: markupsafe==2.1.5 (installed)
DEBUG Tried 27 versions: einops 1, filelock 1, flash-attn 1, fsspec 1, jinja2 1, markupsafe 1, mpmath 1, networkx 1, ninja 1, nvidia-cublas-cu12 1, nvidia-cuda-cupti-cu12 1, nvidia-cuda-nvrtc-cu12 1, nvidia-cuda-runtime-cu12 1, nvidia-cudnn-cu12 1, nvidia-cufft-cu12 1, nvidia-curand-cu12 1, nvidia-cusolver-cu12 1, nvidia-cusparse-cu12 1, nvidia-nccl-cu12 1, nvidia-nvjitlink-cu12 1, nvidia-nvtx-cu12 1, packaging 1, root 1, sympy 1, torch 1, triton 1, typing-extensions 1
Resolved 26 packages in 8ms
DEBUG Requirement already installed: einops==0.8.0
DEBUG Requirement already installed: filelock==3.14.0
DEBUG URL wheel requirement already cached: flash-attn==2.5.8+cu122torch2.3cxx11abifalse (from https://github.com/Dao-AILab/flash-attention/releases/download/v2.5.8/flash_attn-2.5.8+cu122torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl)
DEBUG Requirement already installed: fsspec==2024.3.1
DEBUG Requirement already installed: jinja2==3.1.4
DEBUG Requirement already installed: markupsafe==2.1.5
DEBUG Requirement already installed: mpmath==1.3.0
DEBUG Requirement already installed: networkx==3.3
DEBUG Requirement already installed: ninja==1.11.1.1
DEBUG Requirement already installed: nvidia-cublas-cu12==12.1.3.1
DEBUG Requirement already installed: nvidia-cuda-cupti-cu12==12.1.105
DEBUG Requirement already installed: nvidia-cuda-nvrtc-cu12==12.1.105
DEBUG Requirement already installed: nvidia-cuda-runtime-cu12==12.1.105
DEBUG Requirement already installed: nvidia-cudnn-cu12==8.9.2.26
DEBUG Requirement already installed: nvidia-cufft-cu12==11.0.2.54
DEBUG Requirement already installed: nvidia-curand-cu12==10.3.2.106
DEBUG Requirement already installed: nvidia-cusolver-cu12==11.4.5.107
DEBUG Requirement already installed: nvidia-cusparse-cu12==12.1.0.106
DEBUG Requirement already installed: nvidia-nccl-cu12==2.20.5
DEBUG Requirement already installed: nvidia-nvjitlink-cu12==12.4.127
DEBUG Requirement already installed: nvidia-nvtx-cu12==12.1.105
DEBUG Requirement already installed: packaging==24.0
DEBUG Requirement already installed: sympy==1.12
DEBUG Requirement already installed: torch==2.3.0
DEBUG Requirement already installed: triton==2.3.0
DEBUG Requirement already installed: typing-extensions==4.11.0
DEBUG Unnecessary package: pyyaml==6.0.1
DEBUG Unnecessary package: aiosignal==1.3.1
DEBUG Unnecessary package: argparse==1.4.0
DEBUG Unnecessary package: attrs==23.2.0
DEBUG Unnecessary package: boto3==1.34.105
DEBUG Unnecessary package: botocore==1.34.105
DEBUG Unnecessary package: braceexpand==0.1.7
DEBUG Unnecessary package: certifi==2024.2.2
DEBUG Unnecessary package: charset-normalizer==3.3.2
DEBUG Unnecessary package: click==8.1.7
DEBUG Unnecessary package: frozenlist==1.4.1
DEBUG Unnecessary package: huggingface-hub==0.23.0
DEBUG Unnecessary package: idna==3.7
DEBUG Unnecessary package: jmespath==1.0.1
DEBUG Unnecessary package: jsonschema==4.22.0
DEBUG Unnecessary package: jsonschema-specifications==2023.12.1
DEBUG Unnecessary package: msgpack==1.0.8
DEBUG Unnecessary package: numpy==1.26.4
DEBUG Unnecessary package: pillow==10.3.0
DEBUG Preserving seed package: pip==24.0
DEBUG Unnecessary package: protobuf==5.26.1
DEBUG Unnecessary package: python-dateutil==2.9.0.post0
DEBUG Unnecessary package: ray==2.21.0
DEBUG Unnecessary package: referencing==0.35.1
DEBUG Unnecessary package: regex==2024.5.10
DEBUG Unnecessary package: requests==2.31.0
DEBUG Unnecessary package: rpds-py==0.18.1
DEBUG Unnecessary package: s3transfer==0.10.1
DEBUG Unnecessary package: s5cmd==0.1.0
DEBUG Unnecessary package: safetensors==0.4.3
DEBUG Preserving seed package: setuptools==59.6.0
DEBUG Unnecessary package: six==1.16.0
DEBUG Unnecessary package: tokenizers==0.19.1
DEBUG Unnecessary package: torchvision==0.18.0
DEBUG Unnecessary package: tqdm==4.66.4
DEBUG Unnecessary package: transformers==4.40.2
DEBUG Unnecessary package: urllib3==2.2.1
DEBUG Preserving seed package: uv==0.1.44
DEBUG Unnecessary package: webdataset==0.2.86
DEBUG Preserving seed package: wheel==0.37.1
error: Failed to install: flash_attn-2.5.8+cu122torch2.3cxx11abifalse-cp310-cp310-linux_x86_64.whl (flash-attn==2.5.8+cu122torch2.3cxx11abifalse (from https://github.com/Dao-AILab/flash-attention/releases/download/v2.5.8/flash_attn-2.5.8+cu122torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl))
  Caused by: Wheel version does not match filename: 2.5.8 != 2.5.8+cu122torch2.3cxx11abifalse
  ```
</details>



---

_Comment by @charliermarsh on 2024-05-14 23:04_

We might need to ignore local segments in that comparison.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-14 23:04_

---

_Comment by @charliermarsh on 2024-05-14 23:05_

I assume it works if you rename the file to, like, `flash_attn-2.5.8-cp310-cp310-linux_x86_64.whl`?

---

_Label `compatibility` added by @charliermarsh on 2024-05-14 23:48_

---

_Closed by @charliermarsh on 2024-05-15 00:02_

---

_Comment by @isidentical on 2024-05-15 03:02_

wow, you guys fixed it already 🔥 

---
