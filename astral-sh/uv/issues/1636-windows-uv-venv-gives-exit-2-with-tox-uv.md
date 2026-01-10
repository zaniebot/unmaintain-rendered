---
number: 1636
title: "Windows: \"uv venv\" gives \"exit 2\" with tox-uv"
type: issue
state: closed
author: hugovk
labels:
  - bug
  - windows
  - virtualenv
assignees: []
created_at: 2024-02-18T09:53:20Z
updated_at: 2024-03-05T07:33:38Z
url: https://github.com/astral-sh/uv/issues/1636
synced_at: 2026-01-10T01:23:08Z
---

# Windows: "uv venv" gives "exit 2" with tox-uv

---

_Issue opened by @hugovk on 2024-02-18 09:53_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Windows
uv 0.1.4
tox-uv 1.2.0

Using tox-uv, and running `python -m tox -e py -vvv` on Windows with CPython 3.8-3.13 and PyPy 3.10 gives an error:

```
ROOT: 156 D setup logging to NOTSET on pid 8056 [tox\report.py:221]
py: 375 W venv> C:\hostedtoolcache\windows\Python\3.8.10\x64\uv venv -p 3.8 D:\a\termcolor\termcolor\.tox\py\.venv [tox\tox_env\api.py:427]
py: 375 C exit 2 (0.00 seconds) D:\a\termcolor\termcolor> C:\hostedtoolcache\windows\Python\3.8.10\x64\uv venv -p 3.8 D:\a\termcolor\termcolor\.tox\py\.venv [tox\execute\api.py:280]
  py: FAIL code 2 (0.00 seconds)
  evaluation failed :( (0.22 seconds)
Error: Process completed with exit code 1.
```

It passes on Ubuntu and macOS. I don't have a Windows machine, this is on CI:

https://github.com/hugovk/termcolor/actions/runs/7948092300/job/21697697387

To get the code locally:

```sh
git clone https://github.com/hugovk/termcolor/
git checkout fd9f3f426449541a111560585c4230e31d2e0adf
```

Possibly a tox-uv bug?

---

_Referenced in [tox-dev/tox-uv#15](../../tox-dev/tox-uv/issues/15.md) on 2024-02-18 10:26_

---

_Label `windows` added by @MichaReiser on 2024-02-18 11:55_

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:37_

---

_Comment by @ofek on 2024-02-18 23:06_

https://github.com/astral-sh/uv/issues/1310

---

_Label `bug` added by @zanieb on 2024-02-18 23:59_

---

_Comment by @hugovk on 2024-03-04 19:26_

Reproducible on Windows with uv 0.1.14 for PyPy 3.9 and 3.10, and CPython 3.13 (via tox-uv 1.4.0):

```pytb
Run python -m tox -e py -rvvv
.pkg: 4423 W remove tox env folder D:\a\termcolor\termcolor\.tox\.pkg [tox\tox_env\api.py:325]
py: 4470 W venv> C:\hostedtoolcache\windows\PyPy\3.10.13\x86\Scripts\uv.exe venv -p 3.10 -v D:\a\termcolor\termcolor\.tox\py\.venv [tox\tox_env\api.py:427]
 uv_interpreter::python_query::find_requested_python request=3.10
      0.002894s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python @ `3.10`
      0.003287s   0ms DEBUG uv_interpreter::interpreter Probing interpreter info for: C:\hostedtoolcache\windows\PyPy\3.10.13\x86\python3.10.exe
      0.132933s 130ms DEBUG uv_interpreter::interpreter Found Python 3.10.13 for: C:\hostedtoolcache\windows\PyPy\3.10.13\x86\python3.10.exe
Using Python 3.10.13 interpreter at: C:\hostedtoolcache\windows\PyPy\3.10.13\x86\python3.10.exe
Creating virtualenv at: D:\a\termcolor\termcolor\.tox\py\.venv
uv::venv::creation

  × Failed to create virtualenv
  ├─▶ failed to copy file from
  │   C:\hostedtoolcache\windows\PyPy\3.10.13\x86\Lib\venv\scripts\nt\python.exe
  │   to \\?\D:\a\termcolor\termcolor\.tox\py\.venv\Scripts\python.exe
  ╰─▶ The system cannot find the file specified. (os error 2)
py: 5080 C exit 1 (0.61 seconds) D:\a\termcolor\termcolor> C:\hostedtoolcache\windows\PyPy\3.10.13\x86\Scripts\uv.exe venv -p 3.10 -v D:\a\termcolor\termcolor\.tox\py\.venv pid=6192 [tox\execute\api.py:280]
  py: FAIL code 1 (0.67 seconds)
  evaluation failed :( (4.67 seconds)

Error: Process completed with exit code 1.
```



https://github.com/hugovk/termcolor/actions/runs/8145079624

CPython 3.8-3.12 pass. 

---

_Comment by @charliermarsh on 2024-03-04 19:32_

Thanks, let me see if I can repro with a minimal Python 3.13 run.

---

_Comment by @charliermarsh on 2024-03-04 19:43_

Reproduced here, thanks! https://github.com/astral-sh/uv/pull/2171

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-04 19:43_

---

_Comment by @charliermarsh on 2024-03-04 20:01_

First trying to figure out if we're attempting to copy from the wrong directory, or if we're copying the wrong filename.

---

_Comment by @charliermarsh on 2024-03-04 20:21_

@gaborbernat - Do you know if the development builds exclude the `venv` shims?

---

_Comment by @charliermarsh on 2024-03-04 21:02_

I figured this out, I think we're supposed to fallback to `venvlauncher.exe` if `python.exe` doesn't exist:

https://github.com/python/cpython/blob/d457345bbc6414db0443819290b04a9a4333313d/Lib/venv/__init__.py#L270C66-L270C71

---

_Comment by @charliermarsh on 2024-03-04 21:36_

I asked a question about it on discuss: https://discuss.python.org/t/when-should-venv-scripts-nt-python-exe-be-present/47620

---

_Referenced in [astral-sh/uv#2171](../../astral-sh/uv/pulls/2171.md) on 2024-03-04 21:45_

---

_Closed by @charliermarsh on 2024-03-04 21:49_

---

_Comment by @charliermarsh on 2024-03-04 22:02_

FYI @gaborbernat -- The executable names changed in Python 3.13: https://discuss.python.org/t/when-should-venv-scripts-nt-python-exe-be-present/47620/2. I suspect virtualenv won't _fail_ because it's robust to the shims not existing.

---

_Comment by @hugovk on 2024-03-05 06:31_

Thanks for the CPython 3.13 fix!

This issue is also about PyPy (3.9 and 3.10), does uv need an extra fix there?

---

_Comment by @AlexWaygood on 2024-03-05 07:33_

> This issue is also about PyPy (3.9 and 3.10), does uv need an extra fix there?

Probably — PyPy isn't officially supported right now, though we'd welcome contributions to help us get there! See #2096

---
