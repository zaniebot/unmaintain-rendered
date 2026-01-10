---
number: 7771
title: Nvidia Apex Installation
type: issue
state: closed
author: mishra-tapas
labels:
  - question
assignees: []
created_at: 2024-09-29T01:39:40Z
updated_at: 2025-05-16T01:55:24Z
url: https://github.com/astral-sh/uv/issues/7771
synced_at: 2026-01-10T01:24:19Z
---

# Nvidia Apex Installation

---

_Issue opened by @mishra-tapas on 2024-09-29 01:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I tried installing NVIDIA Apex (https://github.com/NVIDIA/apex) with uv (version 0.4.8) using
"uv pip install -v --no-cache-dir --no-build-isolation --config-settings "--build-option=--cpp_ext" --config-settings "--build-option=--cuda_ext" ./".
However, this does not work: the error i am getting is 'error: unexpected argument '--build-option' found'.
Is there a way around this problem?
Thanks in advance.


---

_Comment by @zanieb on 2024-09-29 15:46_

Can you share the full output?

---

_Label `question` added by @zanieb on 2024-09-29 15:46_

---

_Comment by @BitPhinix on 2024-09-30 21:24_

```
uv pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --config-setting "--build-option=--cpp_ext" --config-setting "--build-option=--cuda_ext" git+https://github.com/NVIDIA/apex
error: unexpected argument '--build-option' found

  tip: a similar argument exists: '--build-isolation'

Usage: uv pip install --verbose... --no-cache <PACKAGE|--requirement <REQUIREMENT>|--editable <EDITABLE>> <--disable-pip-version-check|--user>

For more information, try '--help'.
```

---

_Comment by @BitPhinix on 2024-10-01 01:28_

As a quick fix, you can fork apex and set these args in the setup.py (super hacky fork but enough to give you an idea: https://github.com/NVIDIA/apex/compare/master...BitPhinix:apex:master)

---

_Comment by @zanieb on 2024-10-21 22:01_

When you provide `--config-setting "--build-option=--cpp_ext"` I think your shell will remove the quotes and we will parse it as a flag?

In my shell, the CLI doesn't complain if I do something like this `uv pip install anyio --config-setting '"--build-option=--cpp_ext"'`

---

_Closed by @zanieb on 2024-10-21 22:01_

---

_Comment by @pplmx on 2025-05-16 01:48_

hi, @zanieb 
I’d like to manage the NVIDIA Apex dependency through my `pyproject.toml`. Could you please advise how to configure it correctly?

FYI. This command is okay:
```bash
uv pip install -v \
    --disable-pip-version-check \
    --no-cache-dir \
    --no-build-isolation \
    --config-setting '"--build-option=--cpp_ext"' \
    --config-setting '"--build-option=--cuda_ext"' \
    git+https://github.com/NVIDIA/apex.git@master
```

---
