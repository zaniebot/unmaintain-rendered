---
number: 3098
title: Fails to install torchvision (ROCm)
type: issue
state: closed
author: kaanyalova
labels: []
assignees: []
created_at: 2024-04-17T16:56:14Z
updated_at: 2024-04-17T19:22:40Z
url: https://github.com/astral-sh/uv/issues/3098
synced_at: 2026-01-10T01:23:24Z
---

# Fails to install torchvision (ROCm)

---

_Issue opened by @kaanyalova on 2024-04-17 16:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
uv 0.1.33
Fedora 40 

To reproduce run:
```
uv pip install --pre torchvision  --index-url https://download.pytorch.org/whl/nightly/rocm6.0 
```
Works on pip

Output:
[uv_out.txt](https://github.com/astral-sh/uv/files/15014764/uv_out.txt)




---

_Comment by @zanieb on 2024-04-17 17:19_

Have you tried with `--extra-index-url` instead?

---

_Comment by @kaanyalova on 2024-04-17 18:07_

Still getting the same error

---

_Comment by @charliermarsh on 2024-04-17 18:18_

You probably need to add some local version specifiers in your input requirements. I'd suggest reading through https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers.

---

_Closed by @kaanyalova on 2024-04-17 19:22_

---
