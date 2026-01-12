```yaml
number: 5445
title: Installing into a virtualenv on Windows fails when the location changes
type: issue
state: closed
author: ak-gupta
labels:
  - question
assignees: []
created_at: 2024-07-25T14:44:29Z
updated_at: 2024-07-25T20:57:39Z
url: https://github.com/astral-sh/uv/issues/5445
synced_at: 2026-01-12T15:58:56Z
```

# Installing into a virtualenv on Windows fails when the location changes

---

_@ak-gupta_

Hi there!

`uv` is awesome, but we're running into an issue with installing into a virtualenv on Windows. If I run

```bash
uv pip install --python=/path/to/python
```

I sometimes get an error message similar to the following:

```
RuntimeError: Unable to run the following command: 

 uv pip install --python=C:\Users\RUNNER~1\AppData\Local\Temp\tmp3hg_0oka\.edgetest\core\Scripts\python .[tests]
WARNING  venv:__init__.py:165 Actual environment location may have moved due to redirects, links or junctions.
  Requested location: "C:\Users\RUNNER~1\AppData\Local\Temp\tmp3hg_0oka\.edgetest\lower_env\Scripts\python.exe"
  Actual location:    "C:\Users\runneradmin\AppData\Local\Temp\tmp3hg_0oka\.edgetest\lower_env\Scripts\python.exe"
```

this is running on GitHub Actions using `windows-latest` and `uv` version 0.2.28. If curious, the error log is in the job from [this PR](https://github.com/capitalone/edgetest/pull/81). Any help is greatly appreciated!

---

_Comment by @charliermarsh on 2024-07-25 14:47_

Thanks, that's an interesting one. Those are indeed aliases for one another. Let me see why it's failing...

---

_Comment by @charliermarsh on 2024-07-25 14:51_

In `utils.py`, can you modify the exception to include all of `stdout` and `stderr`? The directory mismatch _may_ be the problem, but it's also just a warning and so not the immediate cause of the failure.

https://github.com/capitalone/edgetest/blob/8ac71cdb26516e2120745934febc724732ab0e8d/edgetest/utils.py#L46

---

_Label `question` added by @charliermarsh on 2024-07-25 14:52_

---

_Comment by @ak-gupta on 2024-07-25 15:52_

Thanks for the quick reply! I updated the logging and got this output:

```
WARNING  venv:__init__.py:158 Actual environment location may have moved due to redirects, links or junctions.
  Requested location: "C:\Users\RUNNER~1\AppData\Local\Temp\tmp3l_vmwg8\.edgetest\core\Scripts\python.exe"
  Actual location:    "C:\Users\runneradmin\AppData\Local\Temp\tmp3l_vmwg8\.edgetest\core\Scripts\python.exe"
ERROR    edgetest.core:core.py:178 Unable to install the local package into core
Traceback (most recent call last):
  File "D:\a\edgetest\edgetest\edgetest\core.py", line 171, in setup
    _run_command(
  File "D:\a\edgetest\edgetest\edgetest\utils.py", line 46, in _run_command
    raise RuntimeError(
RuntimeError: Unable to run the following command: 

 uv pip install --python=C:\Users\RUNNER~1\AppData\Local\Temp\tmp3l_vmwg8\.edgetest\core\Scripts\python .[tests] 

Returned the following stdout: 

  

Returned the following stderr: 

 error: No virtual or system environment found for path `.edgetest\core\Scripts\python`

```

Just to note -- our current code uses `pip` with commands equivalent to

```bash
C:\Users\RUNNER~1\AppData\Local\Temp\tmp3l_vmwg8\.edgetest\core\Scripts\python -m pip install .[tests]
```

since I haven't changed the logic for finding the virtual environment

---

_Comment by @charliermarsh on 2024-07-25 16:28_

Thanks! Just to confirm, is `.edgetest\core` a virtual environment?

---

_Comment by @ak-gupta on 2024-07-25 17:02_

Yes! Essentially our package creates a series of virtual environments, upgrades core dependencies to the latest available version, then runs testing to see if the current library is compatible with upgraded dependencies. For this example, we're using `venv.EnvBuilder(Path.cwd() / ".edgetest" / "core", with_pip=False)`

---

_Comment by @charliermarsh on 2024-07-25 17:21_

I think it needs to be `python.exe`.

---

_Comment by @charliermarsh on 2024-07-25 17:22_

That may not matter here though. One sec.

---

_Comment by @charliermarsh on 2024-07-25 17:25_

Ok yeah tracing the code, I think that might be the problem here.

---

_Comment by @ak-gupta on 2024-07-25 17:26_

Ah. Let me try that!

---

_Comment by @charliermarsh on 2024-07-25 17:26_

We can probably be robust to it, but can you try changing to add the `.exe` extension on your end?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-25 17:27_

---

_Comment by @ak-gupta on 2024-07-25 17:27_

Yep, I'll push up a change and see if that fixes the issue

---

_Comment by @ak-gupta on 2024-07-25 17:44_

The `.exe` extension fixed the issue! Thank you!

---

_Comment by @charliermarsh on 2024-07-25 17:44_

Stellar. @zanieb do you think we should be robust to this? I.e., passing `--python .edgetest\core\Scripts\python` without the `.exe` suffix? (We detect that it's a "file" path, but we later fail to find the file.)

---

_Comment by @charliermarsh on 2024-07-25 18:00_

I'll just fix it, easy and will be a common footgun.

---

_Comment by @zanieb on 2024-07-25 18:01_

Seems okay to infer the `.exe` on Windows, yeah.

---

_Closed by @charliermarsh on 2024-07-25 20:57_

---
