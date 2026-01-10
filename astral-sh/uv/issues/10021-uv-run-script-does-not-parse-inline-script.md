---
number: 10021
title: "`uv run --script -` does not parse inline script metadata"
type: issue
state: closed
author: BarrensZeppelin
labels:
  - bug
assignees: []
created_at: 2024-12-19T09:07:46Z
updated_at: 2024-12-19T18:09:10Z
url: https://github.com/astral-sh/uv/issues/10021
synced_at: 2026-01-10T01:24:49Z
---

# `uv run --script -` does not parse inline script metadata

---

_Issue opened by @BarrensZeppelin on 2024-12-19 09:07_

I would expect that adding the `--script` flag didn't disable reading of inline script metadata from stdin, but it seems to be the case.

```console
$ xsel -b
# /// script
# requires-python = ">=3.10"
# dependencies = [
#     "urllib3",
# ]
# ///

import urllib3
$ xsel -b | uv run -v -
DEBUG uv 0.5.10
Reading inline script metadata from `stdin`
DEBUG Using Python request Python >=3.10 from `requires-python` metadata
DEBUG Searching for Python >=3.10 in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.9-linux-x86_64-gnu`
DEBUG Found `cpython-3.11.9-linux-x86_64-gnu` at `/home/zii/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3.11` (managed installations)
DEBUG Caching via interpreter: `/home/zii/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3.11`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Solving with target Python version: >=3.11.9
DEBUG Adding direct dependency: urllib3*
DEBUG Found stale response for: https://pypi.org/simple/urllib3/
DEBUG Sending revalidation request for: https://pypi.org/simple/urllib3/
DEBUG Found not-modified response for: https://pypi.org/simple/urllib3/
DEBUG Searching for a compatible version of urllib3 (*)
DEBUG Selecting: urllib3==2.2.3 [compatible] (urllib3-2.2.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ce/d9/5f4c13cecde62396b0d3fe530a50ccea91e7dfc1ccf0e09c228841bb5ba8/urllib3-2.2.3-py3-none-any.whl.metadata
DEBUG Tried 1 versions: urllib3 1
DEBUG marker environment resolution took 0.043s
Resolved 1 package in 43ms
DEBUG Assessing Python executable as base candidate: /home/zii/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3.11
DEBUG Using base executable for virtual environment: /home/zii/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3.11
DEBUG Ignoring empty directory
DEBUG Using request timeout of 30s
DEBUG Requirement already cached: urllib3==2.2.3
Installed 1 package in 7ms
 + urllib3==2.2.3
DEBUG Using Python 3.11.9 interpreter at: /home/zii/.cache/uv/archive-v0/BKxFXOwvV9ggZUwFrS0mg/bin/python3
DEBUG Running `python -c`
DEBUG Command exited with code: 0
$ xsel -b | uv run -v --script --isolated -
DEBUG uv 0.5.10
DEBUG No project found; searching for Python interpreter
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.9-linux-x86_64-gnu`
DEBUG Found `cpython-3.11.9-linux-x86_64-gnu` at `/home/zii/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3.11` (managed installations)
DEBUG Creating isolated virtual environment
DEBUG Assessing Python executable as base candidate: /home/zii/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3.11
DEBUG Using base executable for virtual environment: /home/zii/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3.11
DEBUG Ignoring empty directory
DEBUG Using Python 3.11.9 interpreter at: /home/zii/.cache/uv/builds-v0/.tmp7lfpM3/bin/python
DEBUG Running `python -`
Traceback (most recent call last):
  File "<stdin>", line 9, in <module>
ModuleNotFoundError: No module named 'urllib3'
DEBUG Command exited with code: 1
```

---

_Comment by @BarrensZeppelin on 2024-12-19 09:37_

Also, is it not supported to read a script from stdin and pass along command line arguments at the same time?

I can't get this to work:
```console
$ echo '# /// script
     # requires-python = ">=3.10"
     # dependencies = [
     #     "urllib3",
     # ]
     # ///

     import sys; print(sys.argv)' | uv run - 1 2 3
Reading inline script metadata from `stdin`
['-c']
```
Placing a `--` before or after `-` doesn't help either.

---

_Comment by @charliermarsh on 2024-12-19 13:55_

I'm guessing these are bugs.

---

_Label `bug` added by @charliermarsh on 2024-12-19 13:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-19 14:06_

---

_Referenced in [astral-sh/uv#10033](../../astral-sh/uv/issues/10033.md) on 2024-12-19 17:02_

---

_Referenced in [astral-sh/uv#10035](../../astral-sh/uv/pulls/10035.md) on 2024-12-19 17:14_

---

_Closed by @charliermarsh on 2024-12-19 17:52_

---

_Closed by @charliermarsh on 2024-12-19 17:52_

---

_Comment by @BarrensZeppelin on 2024-12-19 18:09_

Thanks! 🎉

---
