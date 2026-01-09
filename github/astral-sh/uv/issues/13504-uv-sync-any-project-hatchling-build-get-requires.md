---
number: 13504
title: "uv sync  any project :hatchling.build.get_requires_for_build_editable"
type: issue
state: closed
author: yanzengyun
labels:
  - question
assignees: []
created_at: 2025-05-17T06:24:39Z
updated_at: 2025-05-18T21:25:39Z
url: https://github.com/astral-sh/uv/issues/13504
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync  any project :hatchling.build.get_requires_for_build_editable

---

_Issue opened by @yanzengyun on 2025-05-17 06:24_

### Question

When running uv sync, I encountered a build error for a local package:


Resolved 158 packages in 3ms  
  x Failed to build `test1 @ file:///D:/pycharm/uv/test1`



  |-> The build backend returned an error  
  `-> Call to `hatchling.build.get_requires_for_build_editable` failed: expected value at line 1 column 1 (exit code: 0)  
      hint: This usually indicates a problem with the package or the build environment.
I've been struggling with this for quite some time. Whether I set the build backend in pyproject.toml to hatchling or setuptools, the build still fails with the same error.

My environment is Windows 11.
The project follows a standard Python package structure, and the pyproject.toml file appears to be correctly configured.

I'm not sure if the issue is with uv, the build backend, or something in my own setup.

What should I do next to diagnose or fix this problem?
Please let me know if you need additional logs or configuration details.

![Image](https://github.com/user-attachments/assets/4de817ba-bfc8-49a6-bcd9-6bd68bf5445d)
![Image](https://github.com/user-attachments/assets/05ebdc77-9f5b-4d5a-9bf9-ddc403fc4a24)

### Platform

windows11

### Version

latest

---

_Label `question` added by @yanzengyun on 2025-05-17 06:24_

---

_Comment by @yanzengyun on 2025-05-17 06:25_

how can i do ,please.  


---

_Comment by @charliermarsh on 2025-05-17 11:24_

Try running `uv init --lib foo` and looking at the way that the project in `foo` is structured. Hatchling has some expectations around the structure, e.g., do you have a subdirectory `test1` or `src/test1`?

---

_Comment by @yanzengyun on 2025-05-17 11:50_

> Try running and looking at the way that the project in is structured. Hatchling has some expectations around the structure, e.g., do you have a subdirectory or ?`uv init --lib foo``foo``test1``src/test1`

![Image](https://github.com/user-attachments/assets/bde7bb7f-804f-4c52-82c5-71b569ce10b1)

I created a project using uv init --lib test1, then ran uv sync, but the problem still exists.

![Image](https://github.com/user-attachments/assets/d6c9db38-6bf1-4abd-8262-a22f4b540108)

I still tried running uv pip install -e ., and the issue persists. but pip install -e . worked


---

_Comment by @charliermarsh on 2025-05-17 11:52_

Do you need to be using that aliyun mirror? If so, you should configure uv to look at it. You can add:

```toml
[[tool.uv.index]]
url = "https://mirrors.aliyun.com/pypi/simple"
default = true
```

To your `pyproject.toml`.

Otherwise, can you create a full reproduction (like a Git repo I can clone) to understand your issue?

---

_Comment by @charliermarsh on 2025-05-18 21:25_

Closing for now, but can re-open with an MRE!

---

_Closed by @charliermarsh on 2025-05-18 21:25_

---
