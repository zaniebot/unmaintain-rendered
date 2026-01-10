```yaml
number: 4188
title: "`uv pip install` does not create `__pycache__`  directories in virtual enviroments"
type: issue
state: closed
author: verveerpj
labels:
  - documentation
assignees: []
created_at: 2024-06-10T07:27:54Z
updated_at: 2024-06-10T12:58:10Z
url: https://github.com/astral-sh/uv/issues/4188
synced_at: 2026-01-10T05:31:37Z
```

# `uv pip install` does not create `__pycache__`  directories in virtual enviroments

---

_Issue opened by @verveerpj on 2024-06-10 07:27_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Steps to reproduce (Linux, Python 3.11, uv 0.2.9):
- `uv venv venv`
- `source .venv/bin/activate`
- `uv pip install numpy` (or any other package)
- There is no `__pycache__` directory in the installation: 

```
# ls -la .venv/lib/python3.11/site-packages/numpy/__pycache__/
ls: cannot access .venv/lib/python3.11/site-packages/numpy/__pycache__/: No such file or directory
```

On our system the installed packages are created writable for all users in the same group. As a result the `__pycache__` folder is created by the first user that imports the packages. This works, but it is created without write permissions for the group and as a result the user that installed the package cannot uninstall it anymore:

- As another user:
```
# source .venv/bin/activate
# python
Python 3.11.4 (main, Apr 12 2024, 11:40:59) [GCC 12.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
```
- As the user that installed the package:
```
# uv pip uninstall numpy
error: failed to remove directory `/home/user/test/.venv/lib/python3.11/site-packages/numpy/__pycache__`
  Caused by: Permission denied (os error 13)
```

Although this may be a permissions issue on our systems, the behavior is different from standard `pip` which creates the `__pycache__` folder upon installations, which avoids the issue.



---

_Comment by @konstin on 2024-06-10 08:11_

We don't compile bytecode on installation by default, you can activate bytecode compilation on installation with `uv pip install --compile-bytecode`

---

_Label `question` added by @konstin on 2024-06-10 08:11_

---

_Comment by @verveerpj on 2024-06-10 08:22_

That should resolve my issue!

Thank you and apologies for not checking the `uv pip` help properly.

The behavior is different from standard `pip` so that bit me. I could not find it in the `PIP_COMPATIBILITY.md`, unless I missed it, would it be worth documenting it there?


---

_Comment by @charliermarsh on 2024-06-10 12:44_

I can add a mention in there.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-10 12:44_

---

_Label `question` removed by @charliermarsh on 2024-06-10 12:44_

---

_Label `documentation` added by @charliermarsh on 2024-06-10 12:44_

---

_Closed by @charliermarsh on 2024-06-10 12:58_

---
