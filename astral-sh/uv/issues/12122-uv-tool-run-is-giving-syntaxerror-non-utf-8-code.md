```yaml
number: 12122
title: "uv tool run is giving \"SyntaxError: Non-UTF-8 code starting with '\\xe8' in file...\""
type: issue
state: open
author: pfmoore
labels:
  - question
assignees: []
created_at: 2025-03-11T22:47:58Z
updated_at: 2025-03-28T12:50:15Z
url: https://github.com/astral-sh/uv/issues/12122
synced_at: 2026-01-10T03:41:47Z
```

# uv tool run is giving "SyntaxError: Non-UTF-8 code starting with '\xe8' in file..."

---

_Issue opened by @pfmoore on 2025-03-11 22:47_

### Question

Whenever I try to run a tool via `uvx` (or `uv tool run`) I get an error:

```
❯ uvx nox --help
  File "C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Scripts\nox.exe", line 1
SyntaxError: Non-UTF-8 code starting with '\xe8' in file C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Scripts\nox.exe on line 2, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

This is happening for (as far as I can see) every tool, and with older versions of uv as well as the current version. I've tested with uv 0.6.5, 0.6.4 and 0.5.30. It doesn't happen with `pipx run`, and it happens with multiple tools (I've tried nox, pdm, and pip) and which suggests that it's something specifically affecting uv.

I'm pretty sure I've used `uvx` before without this error, so while it's likely to be something on my machine that is wrong, I don't know how to diagnose (or fix) the issue. If you can suggest any ways I can work out what's happening, I'd appreciate it.

I'm running Windows 11, with Powershell Core 7.5 as my shell, and Python 3.13 as my default Python interpreter.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @pfmoore on 2025-03-11 22:47_

---

_Comment by @charliermarsh on 2025-03-11 22:49_

I believe this is the error you see when you attempt to use Python 3.7 on Windows with uv-installed executables (https://github.com/astral-sh/uv/issues/11847). Does `uvx --python 3.13 nox --help` or similar do anything differently?

---

_Comment by @pfmoore on 2025-03-11 23:02_

Hmm, yes that fixes it. But why is uv using Python 3.7? I have Python 3.8, 3.9, 3.10, 3.11, 3.12 and 3.13 installed, all via the official python.org installers.

I installed python 3.7 via `uv python install` a few days ago, but it's not my default Python (via `py -V`) and in any case, I wouldn't expect an older version to be preferred over a newer one unless I explicitly asked for that to happen. I see no environment variables that would be relevant, and no config files in my current directory that could affect this. Is there a way to get `uv` to report what config it's using (something similar to `pip config list`)?

Actually, `uv run python` *also* picks Python 3.7 now.

OK, I uninstalled Python 3.7 via `uv python uninstall 3.7` and now I'm back to having Python 3.13 as my default and everything looks fine.

Should I assume that preferring an older but uv-installed Python is a bug, and this is sufficient as a bug report for it, or would you like me to raise a new issue specifically for that problem?

---

_Comment by @pfmoore on 2025-03-11 23:04_

As a further note, even just `uv run --python 3.7 python` (which I would expect to be a purely temporary use of Python 3.7) results in my default being switched to Python 3.7.

---

_Comment by @charliermarsh on 2025-03-11 23:13_

The most useful thing would be if you could share the output of `uvx --verbose nox`, that might give us some hints. But yeah this is totally sufficient as a report. My guess as to what's happening is that we're preferring the Pythons that we installed ourselves, and if you run `uv run --python 3.7 python`, and we can't find Python 3.7, we install it by default; but then we're preferring that managed Python in subsequent executions. @zanieb would know for sure though.

---

_Comment by @powercoconola on 2025-03-11 23:14_

Maybe this would help? You can set default versions.
https://docs.astral.sh/uv/reference/cli/#uv-python-pin

---

_Comment by @powercoconola on 2025-03-11 23:17_

>it's not my default Python (via py -V)

`py -V` is the Python Launcher default. uv doesn't determine your default Python by what the Python Launcher determines is default. It has its own system. You can pin the Python version or use the `UV_PYTHON` environment variable.
https://docs.astral.sh/uv/configuration/environment/#uv_python

---

_Comment by @powercoconola on 2025-03-11 23:26_

>Is there a way to get uv to report what config it's using (something similar to pip config list)?

Maybe try this? `uv python pin`

> If no request is provided, the currently pinned version will be shown.

From: https://docs.astral.sh/uv/reference/cli/#uv-python-pin--request

You can also just look at all UV environment variables to see what the values are.

---

_Comment by @pfmoore on 2025-03-11 23:36_

> The most useful thing would be if you could share the output of uvx --verbose nox, that might give us some hints

Here you go - it does look like it searched for "managed installations" first, and gave up once it found one, without looking at the registry for later versions.

```
DEBUG uv 0.6.5 (bcbcd0a1e 2025-03-06)
DEBUG Searching for default Python interpreter in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\Gustav\AppData\Roaming\uv\python`
DEBUG Found managed installation `cpython-3.13.0rc2-windows-x86_64-none`
DEBUG Found `cpython-3.13.0rc2-windows-x86_64-none` at `C:\Users\Gustav\AppData\Roaming\uv\python\cpython-3.13.0rc2-windows-x86_64-none\python.exe` (managed installations)
DEBUG Skipping pre-release cpython-3.13.0rc2-windows-x86_64-none
DEBUG Found managed installation `cpython-3.7.9-windows-x86_64-none`
DEBUG Found `cpython-3.7.9-windows-x86_64-none` at `C:\Users\Gustav\AppData\Roaming\uv\python\cpython-3.7.9-windows-x86_64-none\python.exe` (managed installations)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Roaming\uv\tools`
DEBUG Checking for Python environment at `C:\Users\Gustav\AppData\Roaming\uv\tools\nox`
DEBUG Released lock at `C:\Users\Gustav\AppData\Roaming\uv\tools\.lock`
DEBUG Caching via base interpreter: `C:\Users\Gustav\AppData\Roaming\uv\python\cpython-3.7.9-windows-x86_64-none\python.exe`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.7.9
DEBUG Solving with target Python version: >=3.7.9
DEBUG Adding direct dependency: nox*
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\nox.lock`
DEBUG Found fresh response for: https://pypi.org/simple/nox/
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\nox.lock`
DEBUG Searching for a compatible version of nox (*)
DEBUG No compatible version found for: Python
DEBUG Recording unit propagation conflict of Python from incompatibility of (nox)
DEBUG Searching for a compatible version of nox (<2025.2.9 | >2025.2.9)
DEBUG Recording unit propagation conflict of Python from incompatibility of (nox)
DEBUG Searching for a compatible version of nox (<2024.10.9 | >2024.10.9, <2025.2.9 | >2025.2.9)
DEBUG Selecting: nox==2024.4.15 [compatible] (nox-2024.4.15-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\nox\nox-2024.4.15-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a4/28/2897c06b54cd99f41ca9e5cc7433211a085903a71aaed1cb1a1dc138d53c/nox-2024.4.15-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\nox\nox-2024.4.15-py3-none-any.lock`
DEBUG Adding transitive dependency for nox==2024.4.15: argcomplete>=1.9.4, <4.0
DEBUG Adding transitive dependency for nox==2024.4.15: colorlog>=2.6.1, <7.0.0
DEBUG Adding transitive dependency for nox==2024.4.15: importlib-metadata{python_full_version < '3.8'}*
DEBUG Adding transitive dependency for nox==2024.4.15: packaging>=20.9
DEBUG Adding transitive dependency for nox==2024.4.15: tomli{python_full_version < '3.11'}>=1
DEBUG Adding transitive dependency for nox==2024.4.15: typing-extensions{python_full_version < '3.8'}>=3.7.4
DEBUG Adding transitive dependency for nox==2024.4.15: virtualenv>=20.14.1
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\argcomplete.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\colorlog.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\importlib-metadata.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\packaging.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\tomli.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\typing-extensions.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\virtualenv.lock`
DEBUG Found fresh response for: https://pypi.org/simple/argcomplete/
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\argcomplete.lock`
DEBUG Found fresh response for: https://pypi.org/simple/colorlog/
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <4.0)
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\colorlog.lock`
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.6.0 | >3.6.0, <4.0)
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\packaging.lock`
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Found fresh response for: https://pypi.org/simple/tomli/
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Package argcomplete has too many conflicts (affected), prioritizing
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\tomli.lock`
DEBUG Package argcomplete has too many conflicts (culprit), already (ConflictEarly(Reverse(1)), PubGrubTiebreaker(Reverse(1)))
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\typing-extensions.lock`
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Found fresh response for: https://pypi.org/simple/importlib-metadata/
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\importlib-metadata.lock`
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Found fresh response for: https://pypi.org/simple/virtualenv/
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\virtualenv.lock`
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (argcomplete)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\colorlog\colorlog-6.9.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\argcomplete\argcomplete-3.1.2-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e3/51/9b208e85196941db2f0654ad0357ca6388ab3ed67efdbfc799f35d1f83aa/colorlog-6.9.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\colorlog\colorlog-6.9.0-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1e/05/223116a4a5905d6b26bff334ffc7b74474fafe23fcb10532caf0ef86ca69/argcomplete-3.1.2-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\argcomplete\argcomplete-3.1.2-py3-none-any.lock`
DEBUG Adding transitive dependency for argcomplete==3.1.2: importlib-metadata{python_full_version < '3.8'}>=0.23, <7
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Adding transitive dependency for colorlog==6.9.0: colorama{sys_platform == 'win32'}*
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <7)
DEBUG Recording unit propagation conflict of Python from incompatibility of (importlib-metadata{python_full_version < '3.8'})
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.11.0 | >6.11.0, <7)
DEBUG Recording unit propagation conflict of Python from incompatibility of (importlib-metadata{python_full_version < '3.8'})
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Recording unit propagation conflict of Python from incompatibility of (importlib-metadata{python_full_version < '3.8'})
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Recording unit propagation conflict of Python from incompatibility of (importlib-metadata{python_full_version < '3.8'})
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Adding transitive dependency for importlib-metadata==6.7.0: importlib-metadata==6.7.0
DEBUG Adding transitive dependency for importlib-metadata==6.7.0: importlib-metadata{python_full_version < '3.8'}==6.7.0
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\colorama.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\importlib-metadata\importlib_metadata-6.7.0-py3-none-any.lock`
DEBUG Found fresh response for: https://pypi.org/simple/colorama/
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\colorama.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ff/94/64287b38c7de4c90683630338cf28f129decbba0a44f0c6db35a873c73c4/importlib_metadata-6.7.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\importlib-metadata\importlib_metadata-6.7.0-py3-none-any.lock`
DEBUG Adding transitive dependency for importlib-metadata==6.7.0: zipp>=0.5
DEBUG Adding transitive dependency for importlib-metadata==6.7.0: typing-extensions{python_full_version < '3.8'}>=3.6.4
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Adding transitive dependency for importlib-metadata==6.7.0: zipp>=0.5
DEBUG Adding transitive dependency for importlib-metadata==6.7.0: typing-extensions{python_full_version < '3.8'}>=3.6.4
DEBUG Searching for a compatible version of packaging (>=20.9)
DEBUG Recording unit propagation conflict of Python from incompatibility of (packaging)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (<6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (importlib-metadata{python_full_version < '3.8'})
DEBUG Package importlib-metadata{python_full_version < '3.8'} has too many conflicts (affected), prioritizing
DEBUG Package importlib-metadata{python_full_version < '3.8'} has too many conflicts (culprit), already (ConflictEarly(Reverse(3)), PubGrubTiebreaker(Reverse(3)))
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.2 | >24.2)
DEBUG Recording unit propagation conflict of Python from incompatibility of (packaging)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\zipp.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\packaging\packaging-24.0-py3-none-any.lock`
DEBUG Found fresh response for: https://pypi.org/simple/zipp/
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\zipp.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/49/df/1fceb2f8900f8639e278b056416d49134fb8d84c5942ffaa01ad34782422/packaging-24.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\packaging\packaging-24.0-py3-none-any.lock`
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1)
DEBUG Recording unit propagation conflict of Python from incompatibility of (tomli{python_full_version < '3.11'})
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.2.1 | >2.2.1)
DEBUG Recording unit propagation conflict of Python from incompatibility of (tomli{python_full_version < '3.11'})
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Recording unit propagation conflict of Python from incompatibility of (tomli{python_full_version < '3.11'})
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Adding transitive dependency for tomli==2.0.1: tomli==2.0.1
DEBUG Adding transitive dependency for tomli==2.0.1: tomli{python_full_version < '3.11'}==2.0.1
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\tomli\tomli-2.0.1-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/97/75/10a9ebee3fd790d20926a90a2547f0bf78f371b2f13aa822c759680ca7b9/tomli-2.0.1-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\tomli\tomli-2.0.1-py3-none-any.lock`
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (typing-extensions{python_full_version < '3.8'})
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.12.2 | >4.12.2)
DEBUG Recording unit propagation conflict of Python from incompatibility of (typing-extensions{python_full_version < '3.8'})
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Recording unit propagation conflict of Python from incompatibility of (typing-extensions{python_full_version < '3.8'})
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Recording unit propagation conflict of Python from incompatibility of (typing-extensions{python_full_version < '3.8'})
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Recording unit propagation conflict of Python from incompatibility of (typing-extensions{python_full_version < '3.8'})
DEBUG Package typing-extensions{python_full_version < '3.8'} has too many conflicts (affected), prioritizing
DEBUG Package typing-extensions{python_full_version < '3.8'} has too many conflicts (culprit), already (ConflictEarly(Reverse(6)), PubGrubTiebreaker(Reverse(6)))
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Recording unit propagation conflict of Python from incompatibility of (typing-extensions{python_full_version < '3.8'})
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Recording unit propagation conflict of Python from incompatibility of (typing-extensions{python_full_version < '3.8'})
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Adding transitive dependency for typing-extensions==4.7.1: typing-extensions==4.7.1
DEBUG Adding transitive dependency for typing-extensions==4.7.1: typing-extensions{python_full_version < '3.8'}==4.7.1
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\typing-extensions\typing_extensions-4.7.1-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ec/6b/63cc3df74987c36fe26157ee12e09e8f9db4de771e0f3404263117e75b95/typing_extensions-4.7.1-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\typing-extensions\typing_extensions-4.7.1-py3-none-any.lock`
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1)
DEBUG Recording unit propagation conflict of Python from incompatibility of (virtualenv)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.29.3 | >20.29.3)
DEBUG Recording unit propagation conflict of Python from incompatibility of (virtualenv)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Recording unit propagation conflict of Python from incompatibility of (virtualenv)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Recording unit propagation conflict of Python from incompatibility of (virtualenv)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Recording unit propagation conflict of Python from incompatibility of (virtualenv)
DEBUG Package virtualenv has too many conflicts (affected), prioritizing
DEBUG Package virtualenv has too many conflicts (culprit), already (ConflictEarly(Reverse(7)), PubGrubTiebreaker(Reverse(7)))
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Recording unit propagation conflict of Python from incompatibility of (virtualenv)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Recording unit propagation conflict of Python from incompatibility of (virtualenv)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Recording unit propagation conflict of Python from incompatibility of (virtualenv)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (>=1, <2.0.2 | >2.0.2, <2.1.0 | >2.1.0, <2.2.1 | >2.2.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of tomli{python_full_version < '3.11'} (==2.0.1)
DEBUG Selecting: tomli==2.0.1 [compatible] (tomli-2.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (>=3.7.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0, <4.11.0 | >4.11.0, <4.12.0 | >4.12.0, <4.12.1 | >4.12.1, <4.12.2 | >4.12.2)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.8'} (==4.7.1)
DEBUG Selecting: typing-extensions==4.7.1 [compatible] (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of argcomplete (>=1.9.4, <3.1.3 | >3.1.3, <3.1.4 | >3.1.4, <3.1.5 | >3.1.5, <3.1.6 | >3.1.6, <3.2.0 | >3.2.0, <3.2.1 | >3.2.1, <3.2.2 | >3.2.2, <3.2.3 | >3.2.3, <3.3.0 | >3.3.0, <3.4.0 | >3.4.0, <3.5.0 | >3.5.0, <3.5.1 | >3.5.1, <3.5.2 | >3.5.2, <3.5.3 | >3.5.3, <3.6.0 | >3.6.0, <4.0)
DEBUG Selecting: argcomplete==3.1.2 [compatible] (argcomplete-3.1.2-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (>=0.23, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\virtualenv\virtualenv-20.26.6-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/59/90/57b8ac0c8a231545adc7698c64c5a36fa7cd8e376c691b9bde877269f2eb/virtualenv-20.26.6-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\virtualenv\virtualenv-20.26.6-py3-none-any.lock`
DEBUG Adding transitive dependency for virtualenv==20.26.6: distlib>=0.3.7, <1
DEBUG Adding transitive dependency for virtualenv==20.26.6: filelock>=3.12.2, <4
DEBUG Adding transitive dependency for virtualenv==20.26.6: importlib-metadata{python_full_version < '3.8'}>=6.6
DEBUG Adding transitive dependency for virtualenv==20.26.6: platformdirs>=3.9.1, <5
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\distlib.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\filelock.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\platformdirs.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\colorama\colorama-0.4.6-py2.py3-none-any.lock`
DEBUG Found fresh response for: https://pypi.org/simple/distlib/
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\distlib.lock`
DEBUG Found fresh response for: https://pypi.org/simple/filelock/
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\filelock.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\colorama\colorama-0.4.6-py2.py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\distlib\distlib-0.3.9-py2.py3-none-any.lock`
DEBUG Found fresh response for: https://pypi.org/simple/platformdirs/
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\simple-v15\pypi\platformdirs.lock`
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/91/a1/cf2472db20f7ce4a6be1253a81cfdf85ad9c7885ffbed7047fb72c24cf87/distlib-0.3.9-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of zipp (>=0.5, <3.21.0 | >3.21.0)
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\distlib\distlib-0.3.9-py2.py3-none-any.lock`
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Package zipp has too many conflicts (affected), prioritizing
DEBUG Package zipp has too many conflicts (culprit), already (ConflictEarly(Reverse(9)), PubGrubTiebreaker(Reverse(11)))
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Recording unit propagation conflict of Python from incompatibility of (zipp)
DEBUG Searching for a compatible version of importlib-metadata (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata{python_full_version < '3.8'} (==6.7.0)
DEBUG Selecting: importlib-metadata==6.7.0 [compatible] (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG Searching for a compatible version of virtualenv (>=20.14.1, <20.27.0 | >20.27.0, <20.27.1 | >20.27.1, <20.28.0 | >20.28.0, <20.28.1 | >20.28.1, <20.29.0 | >20.29.0, <20.29.1 | >20.29.1, <20.29.2 | >20.29.2, <20.29.3 | >20.29.3)
DEBUG Selecting: virtualenv==20.26.6 [compatible] (virtualenv-20.26.6-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\zipp\zipp-3.15.0-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5b/fa/c9e82bbe1af6266adf08afb563905eb87cab83fde00a0a08963510621047/zipp-3.15.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\zipp\zipp-3.15.0-py3-none-any.lock`
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Package filelock has too many conflicts (affected), prioritizing
DEBUG Package filelock has too many conflicts (culprit), already (ConflictEarly(Reverse(11)), PubGrubTiebreaker(Reverse(17)))
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Recording unit propagation conflict of Python from incompatibility of (filelock)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\filelock\filelock-3.12.2-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/45/ec3407adf6f6b5bf867a4462b2b0af27597a26bd3cd6e2534cb6ab029938/filelock-3.12.2-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\filelock\filelock-3.12.2-py3-none-any.lock`
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.3.3 | >4.3.3, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Package platformdirs has too many conflicts (affected), prioritizing
DEBUG Package platformdirs has too many conflicts (culprit), already (ConflictEarly(Reverse(12)), PubGrubTiebreaker(Reverse(18)))
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.3.2 | >4.3.2, <4.3.3 | >4.3.3, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.3.1 | >4.3.1, <4.3.2 | >4.3.2, <4.3.3 | >4.3.3, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.3.0 | >4.3.0, <4.3.1 | >4.3.1, <4.3.2 | >4.3.2, <4.3.3 | >4.3.3, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.2.2 | >4.2.2, <4.3.0 | >4.3.0, <4.3.1 | >4.3.1, <4.3.2 | >4.3.2, <4.3.3 | >4.3.3, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.2.1 | >4.2.1, <4.2.2 | >4.2.2, <4.3.0 | >4.3.0, <4.3.1 | >4.3.1, <4.3.2 | >4.3.2, <4.3.3 | >4.3.3, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.2.0 | >4.2.0, <4.2.1 | >4.2.1, <4.2.2 | >4.2.2, <4.3.0 | >4.3.0, <4.3.1 | >4.3.1, <4.3.2 | >4.3.2, <4.3.3 | >4.3.3, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Recording unit propagation conflict of Python from incompatibility of (platformdirs)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0, <3.18.1 | >3.18.1, <3.18.2 | >3.18.2, <3.19.0 | >3.19.0, <3.19.1 | >3.19.1, <3.19.2 | >3.19.2, <3.19.3 | >3.19.3, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <3.20.2 | >3.20.2, <3.21.0 | >3.21.0)
DEBUG Selecting: zipp==3.15.0 [compatible] (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of filelock (>=3.12.2, <3.12.3 | >3.12.3, <3.12.4 | >3.12.4, <3.13.0 | >3.13.0, <3.13.1 | >3.13.1, <3.13.2 | >3.13.2, <3.13.3 | >3.13.3, <3.13.4 | >3.13.4, <3.14.0 | >3.14.0, <3.15.0 | >3.15.0, <3.15.1 | >3.15.1, <3.15.2 | >3.15.2, <3.15.3 | >3.15.3, <3.15.4 | >3.15.4, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.17.0 | >3.17.0, <4)
DEBUG Selecting: filelock==3.12.2 [compatible] (filelock-3.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of platformdirs (>=3.9.1, <4.1.0 | >4.1.0, <4.2.0 | >4.2.0, <4.2.1 | >4.2.1, <4.2.2 | >4.2.2, <4.3.0 | >4.3.0, <4.3.1 | >4.3.1, <4.3.2 | >4.3.2, <4.3.3 | >4.3.3, <4.3.4 | >4.3.4, <4.3.5 | >4.3.5, <4.3.6 | >4.3.6, <5)
DEBUG Selecting: platformdirs==4.0.0 [compatible] (platformdirs-4.0.0-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\platformdirs\platformdirs-4.0.0-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/16/70be3b725073035aa5fc3229321d06e22e73e3e09f6af78dcfdf16c7636c/platformdirs-4.0.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\Gustav\AppData\Local\uv\cache\wheels-v5\pypi\platformdirs\platformdirs-4.0.0-py3-none-any.lock`
DEBUG Adding transitive dependency for platformdirs==4.0.0: typing-extensions{python_full_version < '3.8'}>=4.7.1
DEBUG Searching for a compatible version of colorlog (>=2.6.1, <7.0.0)
DEBUG Selecting: colorlog==6.9.0 [compatible] (colorlog-6.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20.9, <24.1 | >24.1, <24.2 | >24.2)
DEBUG Selecting: packaging==24.0 [compatible] (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of distlib (>=0.3.7, <1)
DEBUG Selecting: distlib==0.3.9 [compatible] (distlib-0.3.9-py2.py3-none-any.whl)
DEBUG Tried 13 versions: argcomplete 1, colorama 1, colorlog 1, distlib 1, filelock 1, importlib-metadata 1, nox 1, packaging 1, platformdirs 1, tomli 1, typing-extensions 1, virtualenv 1, zipp 1
DEBUG marker environment resolution took 0.026s
Resolved 13 packages in 27ms
DEBUG Checking for Python environment at `C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx`
DEBUG Running `nox`
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\argcomplete-3.1.2.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\colorama-0.4.6.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\colorlog-6.9.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\distlib-0.3.9.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\filelock-3.12.2.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\importlib_metadata-6.7.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\nox-2024.4.15.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\packaging-24.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\platformdirs-4.0.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\tomli-2.0.1.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\typing_extensions-4.7.1.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\virtualenv-20.26.6.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Lib\site-packages\zipp-3.15.0.dist-info
  File "C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Scripts\nox.exe", line 1
SyntaxError: Non-UTF-8 code starting with '\xe8' in file C:\Users\Gustav\AppData\Local\uv\cache\archive-v0\WSO8dwyzdXet_5ZRAQqqx\Scripts\nox.exe on line 2, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
DEBUG Command exited with code: 1
```

---

_Comment by @powercoconola on 2025-03-11 23:54_

I think you are confusing uv-installed Python (managed) and Python installed with the Official Python Installer (system). If you want to prefer versions installed with the Official Python Installer, read: 

https://docs.astral.sh/uv/configuration/environment/#uv_python_preference

uv by default looks for any uv-managed Python installs. Then, if it doesn't find any, it looks for system-installs. If you want to look for system-installs first by default, change the environment variable.


I think what you are asking for with your issue is that the order of preference in which uv looks for versions should be:

uv-managed 3.13
system-installed 3.13
uv-managed 3.12
system-installed 3.12
etc.

This functionality doesn't exist in uv as far as I'm aware.





---

_Comment by @pfmoore on 2025-03-12 09:30_

> I think you are confusing uv-installed Python (managed) and Python installed with the Official Python Installer (system).

I’m not confusing them. I am very well aware of the difference. However, I am saying that it is a very unintuitive behaviour to prefer an older version of Python over a newer one based purely on how it was installed. I think it’s problematic enough to be called a bug (and from his response I thought @charliermarsh agreed). But if it’s not, I’m happy to raise it as an enhancement request to get it changed.

For an example of why the current behaviour is bad, consider a new user who has installed Python 3.13 from python.org. They have heard that uv is awesome, and use it to create virtual environments. One day, they find out about `uv run` and run a script from a colleague using it. Suddenly, their virtual environments start getting created with a weird older version of Python than 3.13, which is (in their mind) what they have installed. What happened? Answer - the script required Python 3.9, and running it changed the Python version that uv preferred, *silently* and *globally*. Sure, the user can set the uv environment variable to use 3.13, but then, when they upgrade Python, everything breaks again.

---

_Comment by @zanieb on 2025-03-12 17:53_

Yeah, I'm aware this default can be confusing. Unfortunately, I think it'd also be confusing if you did `uv python install 3.7` and then we did not use it. We can solve the "silent" change in preference by using a cached Python distribution instead of installing them when needed for an ephemeral environments cc @konstin who is planning on tackling this.

> I am saying that it is a very unintuitive behaviour to prefer an older version of Python over a newer one based purely on how it was installed.

I do think it is reasonable for uv to prefer Python versions it manages over arbitrary ones on the system. I definitely empathize with the confusing user experience of automatic installs being picked up though.

For posterity, some related issues:

- https://github.com/astral-sh/uv/issues/7562
- https://github.com/astral-sh/uv/issues/11039
- https://github.com/astral-sh/uv/issues/5609

---

_Comment by @zanieb on 2025-03-12 17:54_

(I think what you may want is a `python-strategy` option which prefers the latest installed version regardless of its source)

---

_Comment by @pfmoore on 2025-03-12 17:59_

> (I think what you may want is a python-strategy option which prefers the latest installed version regardless of its source)

That would work, yes. I find the current approach borderline-unusably bad (for my personal workflows). So *anything* that lets me say "use the latest non-development Python version you can find, regardless of where it comes from" would be a huge improvement.

Pinning to a specific Python version via something like `UV_PYTHON` isn't the answer, I'm afraid - I had to do that with `py` to overcome its willingness to select development versions, and I found it really annoying when I upgraded and forgot to change the config.

---

_Comment by @zanieb on 2025-03-12 18:04_

That makes sense. Sorry you ran into this rough edge.

The only problem with that mode is it means we can't stop discovery early, we need to query _every_ possible Python interpreter on your system before deciding which one to use. I expect that to be a fairly minor performance hit though, since we cache those queries.

---

_Comment by @pfmoore on 2025-03-12 18:47_

> I think it'd also be confusing if you did uv python install 3.7 and then we did not use it.

Thinking some more about it, wouldn't that happen even now if you had previously done `uv python install 3.13`? The current algorithm isn't "use the latest one installed", it's "decide based on *how* the user installed the interpreter". In my experience, very few people consider how they did the install to be a significant determining factor for anything.

And the way that `uv run <some_script>` (where the script declares a different Python version) can change the default Python interpreter used globally seems particularly unlikely to actually be what users want.

I can understand it would be a breaking change, so not something you can do "just like that" - but do you have any evidence that users actually prefer the current approach[^1]?

[^1]: I imagine it would be hard to find people who would actually be affected - someone who only uses uv to do their installs wouldn't see a difference for example. Same for someone who only uses python.org builds, or someone who only uses one Python version.

---

_Comment by @zanieb on 2025-03-12 19:31_

If you do `uv python install 3.7` and had previously done `uv python install 3.13`, yes we'll prefer the existing 3.13 install. We use the latest installed managed Python interpreter by default. My sentiment there is less that "I expect a specific Python interpreter to be used after it is installed" and more that "If I install a Python interpreter with uv, I expect uv to use it". For example, beginners may use `uv python install` without requesting a specific version.

> And the way that uv run <some_script> (where the script declares a different Python version) can change the default Python interpreter used globally seems particularly unlikely to actually be what users want.

I agree, this is what I suggested we should change (and we happen to be doing some related work that will enable this functionality). It might need to be deferred to a breaking release, I'm not sure yet. I think we could consider it a bug fix, but we usually err on the side of marking these as breaking out of an abundance of caution. I'm certain it will break _someone_'s code .


---

_Comment by @pfmoore on 2025-03-14 12:29_

Thinking some more about this, I'm surprised you don't register your managed builds in the registry, as defined in PEP 514. If you did this, you would be consistent with other builds, and you would also be able to discover all Python interpreters[^1] in a single pass, by scanning the registry.

Obviously this is only applicable on Windows - I don't know how discovery of non-uv installations of Python is handled on Unix.

There doesn't seem to be an issue requesting PEP 514 support. Is it worth me creating one?

[^1]: At least, all PEP 514-compliant interpreters, which *should* be pretty much everything - I don't know if you do extra scans for custom builds, or things like that, which might still be extra work.

---

_Comment by @konstin on 2025-03-14 12:32_

PEP 514 support is implemented, it's still in preview though (`--preview` or `UV_PREVIEW=1`): https://github.com/astral-sh/uv/pull/10634

---

_Comment by @schlamar on 2025-03-28 08:09_

I wonder if the original issue is going to be tackled?

I'm running into this issue as well in a test matrix with Python 3.7 and pytest (`uvx -p 3.7 pytest` reproduces this on Windows).

Edit: Workaround would be a `python -m pytest` for this case, but there might be applications not supporting -m

It's not quite clear what the current stance is about Python 3.7 compatibility. It is not listed in the [platform supported list](https://docs.astral.sh/uv/reference/policies/platforms/). However it is listed in `uv python ls`.


---

_Comment by @charliermarsh on 2025-03-28 12:50_

Yes, we don’t officially support Python 3.7, which is why it’s not included in the list of supported platforms. It mostly works, especially on Linux and macOS, but it’s not officially supported.

---
