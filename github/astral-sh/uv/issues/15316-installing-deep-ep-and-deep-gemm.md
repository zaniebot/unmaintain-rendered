---
number: 15316
title: Installing deep_ep and deep_gemm
type: issue
state: closed
author: vwxyzjn
labels:
  - question
assignees: []
created_at: 2025-08-15T20:28:24Z
updated_at: 2025-12-13T17:00:23Z
url: https://github.com/astral-sh/uv/issues/15316
synced_at: 2026-01-07T13:12:19-06:00
---

# Installing deep_ep and deep_gemm

---

_Issue opened by @vwxyzjn on 2025-08-15 20:28_

### Question

Hi `uv` team,

Was wondering if there are instructions on installing the following packages more easily. Thanks!>

https://github.com/deepseek-ai/DeepEP

https://github.com/deepseek-ai/DeepGEMM



### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @vwxyzjn on 2025-08-15 20:28_

---

_Renamed from "Building deep_ep and deep_gemm" to "Installing deep_ep and deep_gemm" by @vwxyzjn on 2025-08-15 20:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-16 16:15_

---

_Comment by @charliermarsh on 2025-08-16 16:27_

For DeepEP:
```toml
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["deep_ep", "torch"]

[tool.uv.sources]
deep_ep = { git = "https://github.com/deepseek-ai/DeepEP" }

[tool.uv.extra-build-dependencies]
deep_ep = [{ requirement = "torch", match-runtime = true }]
```

For DeepGEMM:
```toml
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["deep_gemm", "torch"]

[tool.uv.sources]
deep_gemm = { git = "https://github.com/deepseek-ai/DeepGEMM" }

[tool.uv.extra-build-dependencies]
deep_gemm = [{ requirement = "torch", match-runtime = true }]
```

And then you can `uv sync` to install.

The only hitch is that you need the CUDA development toolkit (`apt-get -y install cuda-toolkit-12-8` or similar) in order to build these.

I'll be including these in the updated build docs.

---

_Referenced in [astral-sh/uv#15326](../../astral-sh/uv/pulls/15326.md) on 2025-08-16 16:43_

---

_Comment by @vwxyzjn on 2025-08-16 17:45_

You are my hero Charlie! It works! Thanks.

<img width="1303" height="651" alt="Image" src="https://github.com/user-attachments/assets/78ccf841-5980-49d5-883d-a819383171dd" />

---

_Comment by @vwxyzjn on 2025-08-16 21:05_

When running `https://github.com/deepseek-ai/DeepEP/blob/main/tests/test_internode.py` I got 👀

```
ubuntu@costa-dev-worker-0:~/code/thirdparty/test_uv$ uv run test_internode.py
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/torch/_subclasses/functional_tensor.py:279: UserWarning: Failed to initialize NumPy: No module named 'numpy' (Triggered internally at /pytorch/torch/csrc/utils/tensor_numpy.cpp:81.)
  cpu = _conversion_method_template(device=torch.device("cpu"))
Traceback (most recent call last):
  File "/home/ubuntu/code/thirdparty/test_uv/test_internode.py", line 8, in <module>
    import deep_ep
  File "/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/deep_ep/__init__.py", line 3, in <module>
    from .utils import EventOverlap
  File "/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/deep_ep/utils.py", line 8, in <module>
    from deep_ep_cpp import Config, EventHandle
ImportError: /home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/deep_ep_cpp.cpython-312-x86_64-linux-gnu.so: undefined symbol: __cudaRegisterLinkedBinary_52358193_10_runtime_cu_a457eb97
```

---

_Comment by @vwxyzjn on 2025-08-16 21:17_

I think it's because https://github.com/deepseek-ai/DeepEP?tab=readme-ov-file#download-and-install-nvshmem-dependency.

I was able to make progress in 

```bash
[project]
name = "project"
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["deep_ep", "deep_gemm", "torch", "nvidia-nvshmem-cu12"]

[tool.uv.sources]
deep_ep = { git = "https://github.com/deepseek-ai/DeepEP" }
deep_gemm = { git = "https://github.com/deepseek-ai/DeepGEMM" }


[tool.uv.extra-build-dependencies]
deep_ep = [{ requirement = "torch", match-runtime = true }, { requirement = "nvidia-nvshmem-cu12", match-runtime = true }]
deep_gemm = [{ requirement = "torch", match-runtime = true }, { requirement = "nvidia-nvshmem-cu12", match-runtime = true }]
```

Now the error becomes

```
ubuntu@costa-dev-worker-0:~/code/thirdparty/test_uv$ uv run test.py
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/torch/_subclasses/functional_tensor.py:279: UserWarning: Failed to initialize NumPy: No module named 'numpy' (Triggered internally at /pytorch/torch/csrc/utils/tensor_numpy.cpp:81.)
  cpu = _conversion_method_template(device=torch.device("cpu"))
Traceback (most recent call last):
  File "/home/ubuntu/code/thirdparty/test_uv/test.py", line 8, in <module>
    import deep_ep
  File "/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/deep_ep/__init__.py", line 3, in <module>
    from .utils import EventOverlap
  File "/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/deep_ep/utils.py", line 8, in <module>
    from deep_ep_cpp import Config, EventHandle
ImportError: libnvshmem_host.so.3: cannot open shared object file: No such file or directory
```

---

_Comment by @vwxyzjn on 2025-08-16 21:28_

Looks like a bug on the `deep_ep` side somehow:

```
>>> import importlib.util
>>> nvshmem_dir = importlib.util.find_spec("nvidia.nvshmem").submodule_search_locations[0]
/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/nvidia/nvshmem/lib/

ImportError: libnvshmem_host.so.3: cannot open shared object file: No such file or directory

 ls /home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/nvidia/nvshmem/lib/
libnvshmem_device.a               nvshmem_bootstrap_pmi.so.3        nvshmem_bootstrap_uid.so.3        nvshmem_transport_libfabric.so.3
libnvshmem_device.bc              nvshmem_bootstrap_pmi2.so.3       nvshmem_transport_ibdevx.so.3     nvshmem_transport_ucx.so.3
libnvshmem_host.so.3              nvshmem_bootstrap_pmix.so.3       nvshmem_transport_ibgda.so.3
nvshmem_bootstrap_mpi.so.3        nvshmem_bootstrap_shmem.so.3      nvshmem_transport_ibrc.so.3
```

---

_Comment by @vwxyzjn on 2025-08-16 21:33_

On the other hand it appears the `deep_gemm` installation was successful: https://github.com/deepseek-ai/DeepGEMM/blob/main/tests/test_layout.py

<img width="1462" height="495" alt="Image" src="https://github.com/user-attachments/assets/2b15738e-2f71-4c1f-aa80-f6065b6bac09" />

---

_Comment by @vwxyzjn on 2025-08-16 21:43_

```
ubuntu@costa-dev-worker-0:~/code/thirdparty/test_uv$ uv run test_low_latency.py
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
Traceback (most recent call last):
  File "/home/ubuntu/code/thirdparty/test_uv/test_low_latency.py", line 11, in <module>
    import deep_ep
  File "/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/deep_ep/__init__.py", line 3, in <module>
    from .utils import EventOverlap
  File "/home/ubuntu/code/thirdparty/test_uv/.venv/lib/python3.12/site-packages/deep_ep/utils.py", line 8, in <module>
    from deep_ep_cpp import Config, EventHandle
ImportError: libnvshmem_host.so.3: cannot open shared object file: No such file or directory
ubuntu@costa-dev-worker-0:~/code/thirdparty/test_uv$ export NVSHMEM_DIR=$(uv run python -c "import importlib.util; print(importlib.util.find_spec('nvidia.nvshmem').submodule_search_locations[0])")  # Use for DeepEP installation
export LD_LIBRARY_PATH="${NVSHMEM_DIR}/lib:$LD_LIBRARY_PATH"
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
ubuntu@costa-dev-worker-0:~/code/thirdparty/test_uv$
ubuntu@costa-dev-worker-0:~/code/thirdparty/test_uv$ uv run test_low_latency.py
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
```


Whohoo it works. The key is to follow https://github.com/deepseek-ai/DeepEP/blob/main/third-party/README.md#post-installation-configuration. In particular, you'd need to run 

```
export NVSHMEM_DIR=$(uv run python -c "import importlib.util; print(importlib.util.find_spec('nvidia.nvshmem').submodule_search_locations[0])")  # Use for DeepEP installation
export LD_LIBRARY_PATH="${NVSHMEM_DIR}/lib:$LD_LIBRARY_PATH"
uv run test_low_latency.py
```

<img width="1483" height="891" alt="Image" src="https://github.com/user-attachments/assets/f09652ca-9e83-4c28-9e94-a09fe0b86574" />



---

_Comment by @charliermarsh on 2025-08-16 21:50_

Amazing, thanks for following up! I'll incorporate some of this into the docs.

---

_Closed by @zanieb on 2025-08-18 21:15_

---

_Comment by @billxbf on 2025-12-13 16:58_

crazy we got the same problem here @vwxyzjn getting hands on deepseek

---
