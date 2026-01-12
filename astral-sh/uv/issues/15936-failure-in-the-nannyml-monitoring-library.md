```yaml
number: 15936
title: Failure in the Nannyml Monitoring Library Installation
type: issue
state: closed
author: castillouparela
labels:
  - question
assignees: []
created_at: 2025-09-18T20:06:22Z
updated_at: 2025-10-06T18:45:27Z
url: https://github.com/astral-sh/uv/issues/15936
synced_at: 2026-01-12T16:02:20Z
```

# Failure in the Nannyml Monitoring Library Installation

---

_@castillouparela_

### Summary

Hi team,

I've recently started integrating machine learning monitoring tools into my data science project. One of these tools is nannyml, which installs perfectly fine using the traditional method:

> pip install nannyml

However, when I try to add it using uv via:

> uv add nannyml

I encounter the following error:

``
Using CPython 3.11.4 interpreter at: C:\Users\<myuser>\AppData\Local\Programs\Python\Python311\python.exe
Creating virtual environment at: .venv
Resolved 91 packages in 1.68s
error: Distribution `kaleido==0.2.1.post1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
``

``
hint: You're on Windows (`win_amd64`), but `kaleido` (v0.2.1.post1) only has wheels for the following platform: `manylinux2014_armv7l`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
``

I tried pinning the version of kaleido, but the error persists. Even installing the full requirements via uv leads to similar issues. It seems the problem is related to the wheel resolution for kaleido on Windows.

I'm raising this issue in hopes that it can be reviewed and potentially resolved in future versions of uv.

Thanks in advance!

### Platform

Windows 11 x_86_64

### Version

0.8.18

### Python version

3.11.4

---

_Label `bug` added by @castillouparela on 2025-09-18 20:06_

---

_Comment by @zanieb on 2025-09-18 21:06_

What did you pin the version to?

Did you try setting `tool.uv.required-environments` as described in the hint?

You can see they only published a Linux wheel for that version https://inspector.pypi.io/project/kaleido/0.2.1.post1/

In contrast, https://inspector.pypi.io/project/kaleido/0.2.1/ includes a Windows wheel.



---

_Comment by @castillouparela on 2025-09-18 22:51_

Yes, following your recommendation, I was able to successfully install the library using uv by explicitly specifying the operating system where it's being executed. That workaround worked well on my Windows machine.

However, I'm concerned about cross-platform compatibility. Specifically, I'm unsure whether this setup will work when running on Linux machines or when submitting pull requests that trigger CI/CD pipelines using Unix-based agents (e.g., GitHub Actions).

Here’s the pyproject.toml configuration that worked for me:
 ```
[project]
name = "test-nannyml"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "nannyml>=0.13.1",
]
[tool.uv]
required-environments = [
    "sys_platform == 'win32'",
]
```

Let me know if there's a recommended way to make this configuration more portable across platforms, or if there's a best practice for handling such cases in multi-platform environments.

Thanks again for the support!

---

_Comment by @zanieb on 2025-09-18 23:39_

Yes, this should work on Unix still.

`required-environments` just ensures that there are wheels for the listed environments, whereas we'll otherwise select the newest version.



---

_Label `bug` removed by @zanieb on 2025-09-18 23:49_

---

_Label `question` added by @zanieb on 2025-09-18 23:49_

---

_Closed by @castillouparela on 2025-10-06 18:45_

---
