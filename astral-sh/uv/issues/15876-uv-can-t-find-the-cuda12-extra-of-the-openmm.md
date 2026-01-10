---
number: 15876
title: "UV can't find the cuda12 extra of the openmm package"
type: issue
state: closed
author: ahmedselim2017
labels:
  - bug
  - external
assignees: []
created_at: 2025-09-15T16:12:53Z
updated_at: 2025-09-16T10:00:49Z
url: https://github.com/astral-sh/uv/issues/15876
synced_at: 2026-01-10T01:26:01Z
---

# UV can't find the cuda12 extra of the openmm package

---

_Issue opened by @ahmedselim2017 on 2025-09-15 16:12_

### Summary

The addition of the `openmm` package with CUDA support as shown in their [documenataion](https://docs.openmm.org/latest/userguide/application/01_getting_started.html#introduction) (section 3.b.)  doesn't include the CUDA support as `uv add` can't find the `cuda12` extra.

## Minimal Reproduction:
```
uv init openmm_test && cd openmm_test && uv add "openmm[cuda12] " && uv run python -m openmm.testInstallation
```
Output: 
```
Initialized project `test` at `/home/[USERNAME]/tmp/test`
Using CPython 3.12.11
Creating virtual environment at: .venv
Resolved 3 packages in 111ms
warning: The package `openmm==8.3.1` does not have an extra named `cuda12`
warning: The package `openmm==8.3.1` does not have an extra named `cuda12`
Installed 2 packages in 31ms
 + numpy==2.3.3
 + openmm==8.3.1

OpenMM Version: 8.3.1
Git Revision: 6e13f1361dcc3ffb581c7c97e3adfcc61257b813

There are 3 Platforms available:

1 Reference - Successfully computed forces
2 CPU - Successfully computed forces
3 OpenCL - Successfully computed forces

Median difference in forces between platforms:

Reference vs. CPU: 6.28747e-06
Reference vs. OpenCL: 6.75018e-06
CPU vs. OpenCL: 7.64158e-07

All differences are within tolerance.
```

## Expected Behavior

Normally, the `openmm` test also should include a CUDA platform, which can be produced when the package is installed with `pip install "openmm[cuda12]" `:

openMM test output:
```

OpenMM Version: 8.3.1
Git Revision: 6e13f1361dcc3ffb581c7c97e3adfcc61257b813

There are 4 Platforms available:

1 Reference - Successfully computed forces
2 CPU - Successfully computed forces
3 CUDA - Successfully computed forces
4 OpenCL - Successfully computed forces

Median difference in forces between platforms:

Reference vs. CPU: 6.29588e-06
Reference vs. CUDA: 6.75731e-06
CPU vs. CUDA: 7.49266e-07
Reference vs. OpenCL: 6.75018e-06
CPU vs. OpenCL: 7.6695e-07
CUDA vs. OpenCL: 1.81675e-07

All differences are within tolerance.
```

pip output:
```
Collecting openmm[cuda12]
  Downloading openmm-8.3.1-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl.metadata (1.0 kB)
Collecting numpy (from openmm[cuda12])
  Downloading numpy-2.3.3-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl.metadata (62 kB)
Collecting OpenMM-CUDA-12 (from openmm[cuda12])
  Downloading openmm_cuda_12-8.3.1-py3-none-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl.metadata (405 bytes)
Collecting nvidia-cuda-runtime-cu12 (from OpenMM-CUDA-12->openmm[cuda12])
  Downloading nvidia_cuda_runtime_cu12-12.9.79-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (1.7 kB)
Collecting nvidia-cuda-nvcc-cu12 (from OpenMM-CUDA-12->openmm[cuda12])
  Downloading nvidia_cuda_nvcc_cu12-12.9.86-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl.metadata (1.7 kB)
Collecting nvidia-cuda-nvrtc-cu12 (from OpenMM-CUDA-12->openmm[cuda12])
  Downloading nvidia_cuda_nvrtc_cu12-12.9.86-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl.metadata (1.7 kB)
Collecting nvidia-cuda-cupti-cu12 (from OpenMM-CUDA-12->openmm[cuda12])
  Downloading nvidia_cuda_cupti_cu12-12.9.79-py3-none-manylinux_2_25_x86_64.whl.metadata (1.8 kB)
Collecting nvidia-cufft-cu12 (from OpenMM-CUDA-12->openmm[cuda12])
  Downloading nvidia_cufft_cu12-11.4.1.4-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (1.8 kB)
Collecting nvidia-nvjitlink-cu12 (from nvidia-cufft-cu12->OpenMM-CUDA-12->openmm[cuda12])
  Downloading nvidia_nvjitlink_cu12-12.9.86-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl.metadata (1.7 kB)
Downloading openmm-8.3.1-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl (14.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.0/14.0 MB 22.4 MB/s eta 0:00:00
Downloading numpy-2.3.3-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl (16.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 16.6/16.6 MB 22.0 MB/s eta 0:00:00
Downloading openmm_cuda_12-8.3.1-py3-none-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl (2.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 23.4 MB/s eta 0:00:00
Downloading nvidia_cuda_cupti_cu12-12.9.79-py3-none-manylinux_2_25_x86_64.whl (10.8 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 10.8/10.8 MB 21.9 MB/s eta 0:00:00
Downloading nvidia_cuda_nvcc_cu12-12.9.86-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl (40.5 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 40.5/40.5 MB 22.1 MB/s eta 0:00:00
Downloading nvidia_cuda_nvrtc_cu12-12.9.86-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl (89.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 89.6/89.6 MB 22.3 MB/s eta 0:00:00
Downloading nvidia_cuda_runtime_cu12-12.9.79-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (3.5 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.5/3.5 MB 22.6 MB/s eta 0:00:00
Downloading nvidia_cufft_cu12-11.4.1.4-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (200.9 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 200.9/200.9 MB 22.3 MB/s eta 0:00:00
Downloading nvidia_nvjitlink_cu12-12.9.86-py3-none-manylinux2010_x86_64.manylinux_2_12_x86_64.whl (39.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 39.7/39.7 MB 22.6 MB/s eta 0:00:00
Installing collected packages: nvidia-nvjitlink-cu12, nvidia-cuda-runtime-cu12, nvidia-cuda-nvrtc-cu12, nvidia-cuda-nvcc-cu12, nvidia-cuda-cupti-cu12, numpy, openmm, nvidia-cufft-cu12, OpenMM-CUDA-12
  Attempting uninstall: nvidia-nvjitlink-cu12
    Found existing installation: nvidia-nvjitlink-cu12 12.9.86
    Uninstalling nvidia-nvjitlink-cu12-12.9.86:
      Successfully uninstalled nvidia-nvjitlink-cu12-12.9.86
  Attempting uninstall: nvidia-cuda-runtime-cu12
    Found existing installation: nvidia-cuda-runtime-cu12 12.9.79
    Uninstalling nvidia-cuda-runtime-cu12-12.9.79:
      Successfully uninstalled nvidia-cuda-runtime-cu12-12.9.79
  Attempting uninstall: nvidia-cuda-nvrtc-cu12
    Found existing installation: nvidia-cuda-nvrtc-cu12 12.9.86
    Uninstalling nvidia-cuda-nvrtc-cu12-12.9.86:
      Successfully uninstalled nvidia-cuda-nvrtc-cu12-12.9.86
  Attempting uninstall: nvidia-cuda-nvcc-cu12
    Found existing installation: nvidia-cuda-nvcc-cu12 12.9.86
    Uninstalling nvidia-cuda-nvcc-cu12-12.9.86:
      Successfully uninstalled nvidia-cuda-nvcc-cu12-12.9.86
  Attempting uninstall: nvidia-cuda-cupti-cu12
    Found existing installation: nvidia-cuda-cupti-cu12 12.9.79
    Uninstalling nvidia-cuda-cupti-cu12-12.9.79:
      Successfully uninstalled nvidia-cuda-cupti-cu12-12.9.79
  Attempting uninstall: numpy
    Found existing installation: numpy 2.3.3
    Uninstalling numpy-2.3.3:
      Successfully uninstalled numpy-2.3.3
  Attempting uninstall: nvidia-cufft-cu12
    Found existing installation: nvidia-cufft-cu12 11.4.1.4
    Uninstalling nvidia-cufft-cu12-11.4.1.4:
      Successfully uninstalled nvidia-cufft-cu12-11.4.1.4
  Attempting uninstall: OpenMM-CUDA-12
    Found existing installation: OpenMM-CUDA-12 8.3.1
    Uninstalling OpenMM-CUDA-12-8.3.1:
      Successfully uninstalled OpenMM-CUDA-12-8.3.1
Successfully installed OpenMM-CUDA-12-8.3.1 numpy-2.3.3 nvidia-cuda-cupti-cu12-12.9.79 nvidia-cuda-nvcc-cu12-12.9.86 nvidia-cuda-nvrtc-cu12-12.9.86 nvidia-cuda-runtime-cu12-12.9.79 nvidia-cufft-cu12-11.4.1.4 nvidia-nvjitlink-cu12-12.9.86 openmm-8.3.1
```

### Platform

Linux 6.12.43+deb13-amd64 x86_64 GNU/Linux

### Version

uv 0.8.17

### Python version

_No response_

---

_Label `bug` added by @ahmedselim2017 on 2025-09-15 16:12_

---

_Renamed from "UV does not find the cuda12 extra of the openmm package" to "UV can't not find the cuda12 extra of the openmm package" by @ahmedselim2017 on 2025-09-15 16:13_

---

_Renamed from "UV can't not find the cuda12 extra of the openmm package" to "UV can't find the cuda12 extra of the openmm package" by @ahmedselim2017 on 2025-09-15 16:13_

---

_Comment by @konstin on 2025-09-15 19:20_

The problem is that the wheels of openmm 8.3.1 have inconsistent metadata, where the Linux wheel has the cuda12 extra, while the macOS wheel doesn't:
* https://inspector.pypi.io/project/openmm/8.3.1/packages/fe/ff/c64e11fca12c8c51062bcdc9eedfe35a08d3350edc91ddbe3536556afa67/openmm-8.3.1-cp312-cp312-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl/openmm-8.3.1.dist-info/METADATA
* https://inspector.pypi.io/project/openmm/8.3.1/packages/e6/38/40c65c23655885af02a8a26a668d7d45a8b1b528111cbecb03214adc607b/openmm-8.3.1-cp310-cp310-macosx_10_12_x86_64.whl/openmm-8.3.1.dist-info/METADATA

This is a problem in the way openmm builds wheels, uv requires that all wheels have the same `METADATA`. The warning is created by looking at the macOS metadata, which is used for creating the lockfile. You can read more about why uv has this requirement in https://github.com/astral-sh/uv/pull/15683.

---

_Label `external` added by @konstin on 2025-09-15 19:20_

---

_Comment by @ahmedselim2017 on 2025-09-16 10:00_

Thanks, adding `OpenMM-CUDA-12` as another dependency as it is written in the Linux wheel made it pass the CUDA tests.

---

_Closed by @ahmedselim2017 on 2025-09-16 10:00_

---
