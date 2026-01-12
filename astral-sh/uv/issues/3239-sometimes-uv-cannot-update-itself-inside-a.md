```yaml
number: 3239
title: Sometimes uv cannot update itself inside a virtual env (Windows)
type: issue
state: closed
author: inoa-jboliveira
labels:
  - duplicate
assignees: []
created_at: 2024-04-24T14:33:09Z
updated_at: 2024-04-24T15:19:03Z
url: https://github.com/astral-sh/uv/issues/3239
synced_at: 2026-01-12T15:58:42Z
```

# Sometimes uv cannot update itself inside a virtual env (Windows)

---

_@inoa-jboliveira_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
There are some circunstances where uv cannot update itself. I believe I've seen this work before, but currently I am unable to perform the command from inside an env.

- OS: Windows 11
- uv installed 0.1.35, upgrading to 0.1.37
- The env was created by `python -m uv venv`
- I dont have a system `uv` in my PATH outside the env

Virtual env was enabled and the executable is the one running on the virtualenv which means it is replacing the same file that is running.
It is not an issue with admin file permission since `pip install uv` does work on the same shell.

```
uv pip install uv --upgrade
Resolved 1 package in 1.82s
Downloaded 1 package in 2.24s
error: failed to remove file `C:\Data\pproject\env\Lib\site-packages\../../Scripts/uv.exe`
  Caused by: Access is denied. (os error 5)
```

If it is related to the file being open by the process, it would be possible to name it `uv.exe.tmp` and then trigger a new process to rename it?


---

_Comment by @charliermarsh on 2024-04-24 15:03_

I think this is a duplicate of https://github.com/astral-sh/uv/issues/1368.

---

_Closed by @charliermarsh on 2024-04-24 15:03_

---

_Comment by @charliermarsh on 2024-04-24 15:03_

But yeah definitely a bug related to trying to replace the currently-running executable.

---

_Label `duplicate` added by @zanieb on 2024-04-24 15:19_

---
