---
number: 2530
title: Package not importable post install without a process restart
type: issue
state: closed
author: gaborbernat
labels:
  - bug
  - macos
  - installer
assignees: []
created_at: 2024-03-18T23:38:23Z
updated_at: 2024-03-20T00:54:06Z
url: https://github.com/astral-sh/uv/issues/2530
synced_at: 2026-01-10T01:23:18Z
---

# Package not importable post install without a process restart

---

_Issue opened by @gaborbernat on 2024-03-18 23:38_

So this is a weird one that involves the `editables` project, cc @pfmoore for awareness. Originally reported via https://github.com/tox-dev/tox-uv/issues/41, but created a simpler reproducer here. Consider the following test file (first demonstrating with pip) called `magic.py`

```python
from __future__ import annotations

import inspect
import subprocess
import sys

subprocess.check_call([sys.executable, "-m", "pip", "install", "editables"])
from editables import EditableProject

print(inspect.getfile(EditableProject))
```

Which works and does the expected thing:

```bash
❯ virtualenv venv --clear; venv/bin/python magic.py
created virtual environment CPython3.12.2.final.0-64 in 187ms
  creator CPython3Posix(dest=/Users/bgabor8/git/github/tox-uv/venv, clear=True, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, via=copy, app_data_dir=/Users/bgabor8/Library/Application Support/virtualenv)
    added seed packages: pip==24.0
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
Collecting editables
  Using cached editables-0.5-py3-none-any.whl.metadata (3.1 kB)
Using cached editables-0.5-py3-none-any.whl (5.1 kB)
Installing collected packages: editables
Successfully installed editables-0.5
/Users/bgabor8/git/github/tox-uv/venv/lib/python3.12/site-packages/editables/__init__.py
```

When doing the same with `uv`, altered the test file via `magic-uv.py`:

```python
from __future__ import annotations

import inspect
import subprocess

subprocess.check_call(["uv", "pip", "install", "editables"])
from editables import EditableProject

print(inspect.getfile(EditableProject))
```

Which then fails with:

```bash
❯ rm .venv -rf; uv venv; .venv/bin/python magic_uv.py
Using Python 3.12.2 interpreter at: /Users/bgabor8/.pyenv/versions/3.12.2/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate.fish
Resolved 1 package in 1ms
Installed 1 package in 2ms
 + editables==0.5
Traceback (most recent call last):
  File "/Users/bgabor8/git/github/tox-uv/magic_uv.py", line 7, in <module>
    from editables import EditableProject
ModuleNotFoundError: No module named 'editables'
```

It might be macOS only... per the original reporter, but is really weird because is only problem the first run:

```bash
~/git/github/tox-uv on  main [!?⇕]
❯ .venv/bin/python -c 'import editables'
```

---

_Referenced in [tox-dev/tox-uv#41](../../tox-dev/tox-uv/issues/41.md) on 2024-03-18 23:38_

---

_Comment by @pfmoore on 2024-03-19 07:33_

Hmm, yes that’s weird. Other than pointing out that installing new modules into a running Python process is not guaranteed to work, I don’t have any insights.

Does the issue happen with anything other than `editables`? Because there’s no import weirdness if you’re just importing it like this, so I don’t think there’s any reason it’s special in this regard.

---

_Label `installer` added by @AlexWaygood on 2024-03-19 07:54_

---

_Comment by @konstin on 2024-03-19 09:48_

> It might be macOS only

I tried on ubuntu and there it passes

---

_Comment by @pfmoore on 2024-03-19 10:57_

It also works for me on Windows under Python 3.12.2. I'd be inclined to say it's more likely to be the `venv` implementation than the `install` side of things. Does the problem occur on MacOS if you do `rm .venv -rf; virtualenv .venv; .venv/bin/python magic_uv.py`?

---

_Comment by @AlexWaygood on 2024-03-19 12:24_

I can repro this locally on MacOS

---

_Label `macos` added by @AlexWaygood on 2024-03-19 12:24_

---

_Comment by @AlexWaygood on 2024-03-19 12:26_

> Does the problem occur on MacOS if you do `rm .venv -rf; virtualenv .venv; .venv/bin/python magic_uv.py`?

it still reproduces for me if I delete the `uv`-created venv and create a new venv using `python -m venv` rather than `uv venv`

---

_Comment by @AlexWaygood on 2024-03-19 12:30_

> Does the issue happen with anything other than `editables`? Because there’s no import weirdness if you’re just importing it like this, so I don’t think there’s any reason it’s special in this regard.

It doesn't seem to be `editables`-specific; I can also repro it if I try to dynamically install `requests` via a subprocess and then import it later in the same script

---

_Comment by @gaborbernat on 2024-03-19 13:54_

> > Does the problem occur on MacOS if you do `rm .venv -rf; virtualenv .venv; .venv/bin/python magic_uv.py`?
> 
> it still reproduces for me if I delete the `uv`-created venv and create a new venv using `python -m venv` rather than `uv venv`

Does it also happen with virtualenv? 

---

_Comment by @gaborbernat on 2024-03-19 13:55_

> Hmm, yes that’s weird. Other than pointing out that installing new modules into a running Python process is not guaranteed to work, I don’t have any insights.

@pfmoore I don't think I read anywhere anything about this, so I would expect that there would be no reason for this not to work. Can you point me to some documentation telling otherwise? 



---

_Comment by @zanieb on 2024-03-19 14:49_

Weird... I can also reproduce on macOS. Does `pip` reset some sort of interpreter module cache on install? 

This succeeds with `uv venv --seed; .venv/bin/python example.py` and `python -m pip install ...` within so I think it's not a virtual environment implementation difference.

---

_Comment by @zanieb on 2024-03-19 14:58_

Adding a `importlib.invalidate_caches()` call after the install makes this pass. 

Adding 
```
subprocess.check_call(
    [sys.executable, "-m", "pip", "install", "editables", "-v", "--force-reinstall"],
    env=env,
)
```
 after the `uv` install also makes this pass, but without reinstall it fails still.

---

_Comment by @pfmoore on 2024-03-19 16:23_

> @pfmoore I don't think I read anywhere anything about this, so I would expect that there would be no reason for this not to work. Can you point me to some documentation telling otherwise?

As @zanieb pointed out, `importlib.invalidate_caches()` is likely important here. The import system has a bunch of logic that (for speed) captures the state of the filesystem when it first encounters it. Installing new stuff into the environment of a running program means that "new stuff" might not be cached and might therefore not be importable without some effort - and the easiest, and cleanest, way to reset the relevant stored state is to restart the process.

The relevant documentation is the [importlib documentation](https://docs.python.org/3.12/library/importlib.html#importlib.invalidate_caches) for `invalidate_caches`, and the caveats in the following section on `reload` are relevant as well. Pip's documentation also briefly [covers](https://pip.pypa.io/en/stable/user_guide/#using-pip-from-your-program) this:

> It should also be noted that installing packages into `sys.path` in a running Python process is something that should only be done with care. The import system caches certain data, and installing new packages while a program is running may not always behave as expected. In practice, there is rarely an issue, but it is something to be aware of.

That's little more than us saying "we don't guarantee this works, but it's not our fault" though...

---

_Comment by @gaborbernat on 2024-03-19 16:48_

That doesn't explain why this case works for pip and not uv though 😆 

---

_Comment by @ofek on 2024-03-19 16:57_

Also why only macOS?

---

_Comment by @pfmoore on 2024-03-19 17:03_

> That doesn't explain why this case works for pip and not uv though 😆

Maybe because uv is hard linking stuff from the cache, so file timestamps are different? I'm just guessing, though, I don't see why that would only matter on macOS. Does uv use a different link mode on macOS? Or does macOS handle hard links differently than other operating systems somehow?

---

_Comment by @charliermarsh on 2024-03-19 17:03_

Sorry to chime in without having read the thread, but FWIW on macOS we use reflinks (Copy-on-Write). On Linux we use hardlinks. (These are just the defaults.)

---

_Comment by @zanieb on 2024-03-19 17:10_

I've narrowed this down to a difference in `mtime` — the `mtime` of the `site-packages` directory changes for `pip` but not for `uv` so the cache is not invalidated.

---

_Comment by @zanieb on 2024-03-19 17:16_

```python
import subprocess
import sys
import os


def get_importer_mtimes() -> dict:
    """
    Get a mapping of directory to modified times for each directory in the sys path importer cache.
    """
    result = {}
    for key, finder in sys.path_importer_cache.items():
        if finder:
            mtime = os.stat(key).st_mtime
            result[key] = mtime
    return result


baselines = get_importer_mtimes()
subprocess.check_call(
    [
        "uv",
        "pip",
        "install",
        "editables",
        "-v",
    ],
)

uv = get_importer_mtimes()

subprocess.check_call(
    [sys.executable, "-m", "pip", "install", "editables", "-v", "--force-reinstall"],
)
pip = get_importer_mtimes()

for key, baseline in baselines.items():
    uv_diff = baseline - uv[key]
    pip_diff = baseline - pip[key]
    print("uv:", uv_diff, "pip:", pip_diff, "path:", key)
    if uv_diff or pip_diff:
        print("baseline:", baseline, "raw uv:", uv[key], "raw pip:", pip[key])
```

```
uv: 0.0 pip: -0.9294626712799072 path: /Users/mz/workspace/uv/.venv/lib/python3.12/site-packages
baseline: 1710868563.3657737 raw uv: 1710868563.3657737 raw pip: 1710868564.2952363
```

---

_Comment by @zanieb on 2024-03-19 17:19_

Basically... we need to make sure the mtime changes when we install

---

_Label `bug` added by @zanieb on 2024-03-19 17:19_

---

_Assigned to @zanieb by @zanieb on 2024-03-19 17:34_

---

_Comment by @pfmoore on 2024-03-19 17:48_

> Sorry to chime in without having read the thread, but FWIW on macOS we use reflinks 

Thanks - I couldn't find any docs that stated what the defaults were on different platforms but I wondered if that might be a relevant difference.

---

_Referenced in [astral-sh/uv#2545](../../astral-sh/uv/pulls/2545.md) on 2024-03-19 17:50_

---

_Closed by @zanieb on 2024-03-20 00:54_

---
