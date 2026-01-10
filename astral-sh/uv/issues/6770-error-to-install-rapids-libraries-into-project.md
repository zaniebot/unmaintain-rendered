---
number: 6770
title: Error to install RAPIDS libraries into project
type: issue
state: closed
author: lucaskbobadilla
labels: []
assignees: []
created_at: 2024-08-28T18:32:14Z
updated_at: 2025-03-02T03:33:09Z
url: https://github.com/astral-sh/uv/issues/6770
synced_at: 2026-01-10T01:24:05Z
---

# Error to install RAPIDS libraries into project

---

_Issue opened by @lucaskbobadilla on 2024-08-28 18:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.


* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I am trying to install the accelarated pandas version from rapids but I am getting the following error:

- If I try to use: ` uv add cudf-cu12`

I am getting the following error:

```
Building cudf-cu12==24.8.2
⠼ cudf-cu12==24.8.2                                                                                                                                                                                                                                                             error: Failed to download and build `cudf-cu12==24.8.2`
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:

--- stderr:

nvidia_stub.error.InstallFailedError:
*******************************************************************************

The installation of cudf-cu12 for version 24.8.2 failed.

This is a special placeholder package which downloads a real wheel package
from https://pypi.nvidia.com. If https://pypi.nvidia.com is not reachable, we
cannot download the real wheel file to install.

You might try installing this package via

$ pip install --extra-index-url https://pypi.nvidia.com cudf-cu12


Here is some debug information about your platform to include in any bug
report:

Python Version: CPython 3.12.5
Operating System: Linux 4.18.0-477.27.1.el8_8.x86_64
CPU Architecture: x86_64
nvidia-smi command not found. Ensure NVIDIA drivers are installed.

*******************************************************************************
---
```

- I tried to use pip too as suggested in the error: `uv pip install --extra-index-url https://pypi.nvidia.com cudf-cu12`

```
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of cudf-cu12 are available:
          cudf-cu12==23.6.0
          cudf-cu12==23.6.1
          cudf-cu12==23.8.0
          cudf-cu12==23.10.0
          cudf-cu12==23.10.1
          cudf-cu12==23.10.2
          cudf-cu12==23.12.0
          cudf-cu12==23.12.1
          cudf-cu12==24.2.0
          cudf-cu12==24.2.1
          cudf-cu12==24.2.2
          cudf-cu12==24.4.0
          cudf-cu12==24.4.1
          cudf-cu12==24.6.0
          cudf-cu12==24.6.1
          cudf-cu12==24.8.1
          cudf-cu12==24.8.2
      and all versions of cudf-cu12 have no wheels with a matching Python ABI tag, we can conclude that cudf-cu12<23.6.1 cannot be used.
      And because you require cudf-cu12, we can conclude that your requirements are unsatisfiable.
```

**uv version:** 0.40

---

_Comment by @zanieb on 2024-08-28 18:34_

Hi! The error is from the package build (as well as the hint about using pip). I think the relevant line is:

```
nvidia-smi command not found. Ensure NVIDIA drivers are installed.
```

Do you have that command?

---

_Comment by @lucaskbobadilla on 2024-08-28 18:59_

Yeah, I was not activating CUDA before. I activate it now to have `nvidia-smi` available but the error still persists. I also noticed some SSL error from uv requests. Is there a way to update the certificates uv can access?


---

_Comment by @zanieb on 2024-08-28 19:07_

@lucaskbobadilla you can use `--native-tls` to use your system certificates.



---

_Comment by @abc8747 on 2024-10-07 12:17_

Python 3.12 is not yet officially supported in the stable channel - switching to the pre-release version worked for me:
`pyproject.toml`
```toml
[project]
name = ""
version = ""
description = ""
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
  "cudf-cu12"
]

[tool.uv]
extra-index-url = ["https://pypi.anaconda.org/rapidsai-wheels-nightly/simple"]
index-strategy = "unsafe-best-match"
prerelease = "allow"
```

---

_Comment by @lucaskbobadilla on 2024-10-07 14:57_

@cathaypacific8747 That worked for me

---

_Closed by @charliermarsh on 2025-03-02 03:33_

---
