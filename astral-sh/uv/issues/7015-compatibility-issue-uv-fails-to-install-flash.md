```yaml
number: 7015
title: "[compatibility issue] uv fails to install flash-attn==2.6.1"
type: issue
state: closed
author: mobidyc
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-09-04T13:15:57Z
updated_at: 2024-09-04T13:47:00Z
url: https://github.com/astral-sh/uv/issues/7015
synced_at: 2026-01-12T15:59:09Z
```

# [compatibility issue] uv fails to install flash-attn==2.6.1

---

_@mobidyc_

```shell
# uv --version
uv 0.4.4

# pip --version
pip 24.0 from /usr/local/lib/python3.10/dist-packages/pip (python 3.10)

# python --version
Python 3.10.12

# grep VERSION= /etc/os-release 
VERSION="22.04.4 LTS (Jammy Jellyfish)"
```

Intall arch: `linux/amd64 using Dockerfile`
Base os : `nvcr.io/nvidia/pytorch:24.06-py3`

uv code in error:
```shell
# uv pip install --system flash-attn==2.6.1
Resolved 24 packages in 799ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: flash-attn==2.6.1
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/root/.cache/uv/builds-v0/.tmpAjfIR6/lib/python3.10/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
  File "/root/.cache/uv/builds-v0/.tmpAjfIR6/lib/python3.10/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/root/.cache/uv/builds-v0/.tmpAjfIR6/lib/python3.10/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/root/.cache/uv/builds-v0/.tmpAjfIR6/lib/python3.10/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 19, in <module>
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that flash-attn==2.6.1 depends on torch, but doesn't declare it as a build dependency. If flash-attn==2.6.1 is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
```

pip code in success:
```shell
# pip install flash-attn==2.6.1
Looking in indexes: https://pypi.org/simple, https://pypi.ngc.nvidia.com
Collecting flash-attn==2.6.1
  Downloading flash_attn-2.6.1.tar.gz (2.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.6/2.6 MB 20.3 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Requirement already satisfied: torch in /usr/local/lib/python3.10/dist-packages (from flash-attn==2.6.1) (2.4.0)
Collecting einops (from flash-attn==2.6.1)
  Downloading einops-0.8.0-py3-none-any.whl.metadata (12 kB)
Requirement already satisfied: filelock in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (3.15.4)
Requirement already satisfied: typing-extensions>=4.8.0 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (4.12.2)
Requirement already satisfied: sympy in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (1.13.2)
Requirement already satisfied: networkx in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (3.3)
Requirement already satisfied: jinja2 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (3.1.4)
Requirement already satisfied: fsspec in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (2024.6.1)
Requirement already satisfied: nvidia-cuda-nvrtc-cu12==12.1.105 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (12.1.105)
Requirement already satisfied: nvidia-cuda-runtime-cu12==12.1.105 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (12.1.105)
Requirement already satisfied: nvidia-cuda-cupti-cu12==12.1.105 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (12.1.105)
Requirement already satisfied: nvidia-cudnn-cu12==9.1.0.70 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (9.1.0.70)
Requirement already satisfied: nvidia-cublas-cu12==12.1.3.1 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (12.1.3.1)
Requirement already satisfied: nvidia-cufft-cu12==11.0.2.54 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (11.0.2.54)
Requirement already satisfied: nvidia-curand-cu12==10.3.2.106 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (10.3.2.106)
Requirement already satisfied: nvidia-cusolver-cu12==11.4.5.107 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (11.4.5.107)
Requirement already satisfied: nvidia-cusparse-cu12==12.1.0.106 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (12.1.0.106)
Requirement already satisfied: nvidia-nccl-cu12==2.20.5 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (2.20.5)
Requirement already satisfied: nvidia-nvtx-cu12==12.1.105 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (12.1.105)
Requirement already satisfied: triton==3.0.0 in /usr/local/lib/python3.10/dist-packages (from torch->flash-attn==2.6.1) (3.0.0)
Requirement already satisfied: nvidia-nvjitlink-cu12 in /usr/local/lib/python3.10/dist-packages (from nvidia-cusolver-cu12==11.4.5.107->torch->flash-attn==2.6.1) (12.6.68)
Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib/python3.10/dist-packages (from jinja2->torch->flash-attn==2.6.1) (2.1.5)
Requirement already satisfied: mpmath<1.4,>=1.1.0 in /usr/local/lib/python3.10/dist-packages (from sympy->torch->flash-attn==2.6.1) (1.3.0)
Downloading einops-0.8.0-py3-none-any.whl (43 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 43.2/43.2 kB 180.8 MB/s eta 0:00:00
Building wheels for collected packages: flash-attn
  Building wheel for flash-attn (setup.py) ... done
  Created wheel for flash-attn: filename=flash_attn-2.6.1-cp310-cp310-linux_x86_64.whl size=198462964 sha256=f4677246da9ecab85d24e9f958388997fccf371a6af8d7d02c294661e41aabd1
  Stored in directory: /tmp/pip-ephem-wheel-cache-fts6l0iu/wheels/91/6a/38/f0faa036b4ac73a73247386f1ab1bb4cb4f6e72e6861a779f1
Successfully built flash-attn
Installing collected packages: einops, flash-attn
Successfully installed einops-0.8.0 flash-attn-2.6.1
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```


---

_Comment by @zanieb on 2024-09-04 13:27_

Please see https://github.com/astral-sh/uv/issues/2252 and https://docs.astral.sh/uv/pip/compatibility/#pep-517-build-isolation

---

_Label `question` added by @charliermarsh on 2024-09-04 13:28_

---

_Label `duplicate` added by @zanieb on 2024-09-04 13:28_

---

_Comment by @mobidyc on 2024-09-04 13:47_

Thank you, it works well.

```shell
uv pip install --system vllm==0.5.5 pynvml==11.5.0
uv pip install --system --no-build-isolation flash-attn==2.6.1 git+https://github.com/Dao-AILab/flash-attention@v2.6.1#subdirectory=csrc/fused_dense_lib
```

---

_Closed by @mobidyc on 2024-09-04 13:47_

---
