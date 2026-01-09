---
number: 1310
title: "Windows: uv doesn't find python versions on PATH"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-15T08:37:03Z
updated_at: 2024-10-08T15:14:50Z
url: https://github.com/astral-sh/uv/issues/1310
synced_at: 2026-01-07T13:12:16-06:00
---

# Windows: uv doesn't find python versions on PATH

---

_Issue opened by @MichaReiser on 2024-02-15 08:37_

I installed Python using pyenv-win but puffin can't find it (because it's not in the PATH). Running python from the command line works as expected and starship recognises it correctly too. 

```bash
❯ python -V
Python 3.12.0

puffin on  main [?] is 📦 v0.0.4 via 🐍 v3.12.0 via 🦀 v1.75.0
❯ where.exe python
C:\Users\Micha\.pyenv\pyenv-win\shims\python
C:\Users\Micha\.pyenv\pyenv-win\shims\python.bat
C:\Users\Micha\AppData\Local\Microsoft\WindowsApps\python.exe

py --list-paths
 -V:IronPython/3.4 * C:\Program Files\IronPython 3.4\ipy.exe
 -V:IronPython/3.4-32 C:\Program Files\IronPython 3.4\ipy32.exe

puffin venv
  × Could not find `python.exe` in PATH and `py --list-paths` did not list any Python versions. Is Python installed?
```

Probably related to https://github.com/astral-sh/puffin/issues/1168


Work around: use `pyenv which python` and pass the path explicitly `puffin pip -p <path_shown_by_pyenv_which>`

---

_Label `windows` added by @MichaReiser on 2024-02-15 08:37_

---

_Comment by @charliermarsh on 2024-02-15 14:55_

Nice, good find.

---

_Comment by @MichaReiser on 2024-02-16 19:16_

The issue is probably more general and applies to all python versions that are on the PATH. See [this comment](https://github.com/astral-sh/uv/pull/940#issuecomment-1949127576)

---

_Referenced in [astral-sh/uv#940](../../astral-sh/uv/pulls/940.md) on 2024-02-16 19:16_

---

_Renamed from "Windows: Doesn't find python version installed with pyenv-win" to "Windows: uv doesn't find python versions on PATH" by @MichaReiser on 2024-02-16 19:16_

---

_Label `bug` added by @zanieb on 2024-02-16 19:32_

---

_Comment by @gaborbernat on 2024-02-16 20:38_

Ran into this on Github Actions too, see https://github.com/tox-dev/tox-uv/actions/runs/7935511069?pr=2

---

_Comment by @melMass on 2024-02-17 17:17_

Yep, uv seems to expect the global `py` (apparently a thing on windows)
I also use pyenv-win and pyenv on unix and have the same issue:

```sh
❯ uv venv
  × Failed to run `py --list-paths` to find Python installations. Is Python installed?
  ╰─▶ program not found
  ```
  
  I tried all installation methods (cargo, pip, from source)

---

_Comment by @zanieb on 2024-02-17 18:00_

@melMass is `python.exe` on your `PATH`?

---

_Comment by @melMass on 2024-02-17 18:03_

> @melMass is `python.exe` on your `PATH`?

Yep
<img src="https://github.com/astral-sh/uv/assets/7041726/2ec6ea87-69c7-4d0a-9003-1a659380e303" width=400/>

Maybe the `bat` shims is causing an issue?

Specifying the path like this works fine:

```sh
uv venv -p (pyenv exec python -c "import sys; print(sys.executable)")
```

![image](https://github.com/astral-sh/uv/assets/7041726/7c8b15f0-9518-48df-b8e1-82fa1d5ab236)


---

_Comment by @zanieb on 2024-02-17 18:05_

It looks like `python` and `python.bat` are on your path, we don't support those shims yet. You should add `python.exe` to your path e.g. by adding the `sys.executable` folder you showed at the end to `PATH`.

Or, yes, you can use the full path to the interpreter executable directly

---

_Comment by @melMass on 2024-02-17 18:07_

> It looks like `python` and `python.bat` are on your path, we don't support those shims yet. You should add `python.exe` to your path e.g. by adding the `sys.executable` folder you showed at the end to `PATH`.

Thanks, it makes sense.

`pyenv-win` is a little finicky so I don't mind specifying the full path, I avoid global pythons at all cost, from experience it creates complex issues to debug (especially on windows)

In this case I think I'll use the cargo releases to avoid `pip` & `pipx` being responsible of uv

---

_Comment by @MichaReiser on 2024-02-17 19:18_

I plan to work on this on Monday except someone beats me to it. Appreciate any pointers.

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-17 19:18_

---

_Comment by @zanieb on 2024-02-17 19:21_

We _do_ correctly find `python.exe` with #1381. I think #1521 is the next major step for Windows Python discovery. I'm not sure if we have intermediate solutions, but we could maybe support `python` shims from pyenv-win like we do for pyenv on unix?

---

_Comment by @gaborbernat on 2024-02-17 19:41_

@zanieb any idea why GitHub actions fails?

https://github.com/astral-sh/uv/issues/1521#issuecomment-1949299848

---

_Comment by @zanieb on 2024-02-17 20:23_

Hm this looks to be working in GitHub actions with `setup-python` over here https://github.com/astral-sh/uv/actions/runs/7944040120/job/21689087415?pr=1611

---

_Comment by @ofek on 2024-02-18 22:25_

https://github.com/astral-sh/uv/issues/1521 should not be the next step. I know that is a spec and I know that virtualenv supports that, but I guarantee you that it is generally unused in most scenarios and you should prioritize implementing lookups based on PATH. The way virtualenv works is that it will try the registry and if the result is not empty then use PATH but in all of my time in enterprise and on my personal machine it has always responded to modifications to PATH.

---

_Referenced in [astral-sh/uv#1636](../../astral-sh/uv/issues/1636.md) on 2024-02-18 23:06_

---

_Referenced in [astral-sh/uv#1639](../../astral-sh/uv/issues/1639.md) on 2024-02-18 23:08_

---

_Referenced in [astral-sh/uv#1660](../../astral-sh/uv/issues/1660.md) on 2024-02-18 23:13_

---

_Comment by @zanieb on 2024-02-18 23:58_

@ofek can you please clarify what you'd like us to find on the PATH? We already will find `python.exe`.

---

_Comment by @gaborbernat on 2024-02-18 23:59_

> Hm this looks to be working in GitHub actions with `setup-python` over here [astral-sh/uv/actions/runs/7944040120/job/21689087415?pr=1611](https://github.com/astral-sh/uv/actions/runs/7944040120/job/21689087415?pr=1611)

When called from within tox https://github.com/tox-dev/tox-uv/pull/19 seems it fails though.... 


---

_Comment by @gaborbernat on 2024-02-19 00:36_

@zanieb can you help what information exactly is used on Windows to create the environment today? Env-vars, and such 😊 

---

_Comment by @ofek on 2024-02-19 01:03_

@zanieb Yes I understand that but the issue is that you're not finding every single `python.exe` on PATH.

![image](https://github.com/astral-sh/uv/assets/9677399/d932e53d-c699-425a-a790-386a28302f36)


---

_Comment by @MichaReiser on 2024-02-19 10:42_

> > Hm this looks to be working in GitHub actions with `setup-python` over here [astral-sh/uv/actions/runs/7944040120/job/21689087415?pr=1611](https://github.com/astral-sh/uv/actions/runs/7944040120/job/21689087415?pr=1611)
> 
> When called from within tox [tox-dev/tox-uv#19](https://github.com/tox-dev/tox-uv/pull/19) seems it fails though....

This seems unrelated to the Python discovery because `uv` successfully starts to resovle dependencies but fails with `Unable to extract filename from URL: d:\a\tox-uv\tox-uv\.tox\.tmp\package\1\tox_uv-0.1.dev1+g5039064-py3-none-any.whl`

---

_Comment by @MichaReiser on 2024-02-19 10:56_

According to pep514, `py` already makes use of the registry information. Therefore, implementing PEP514 is unlikely to help when trying to discover all python versions.

> When installed on Windows, the official Python installer creates a registry key for discovery and detection by other applications. This allows tools such as installers or IDEs to automatically detect and display a user’s Python installations. For example, the [PEP 397](https://peps.python.org/pep-0397/) py.exe launcher and editors such as PyCharm and Visual Studio already make use of this information.

Virtualenv has a fallback where it iterates over each path and tries to find the right python version https://github.com/pypa/virtualenv/blob/fa283474fd199e3836f8b2c99510190c6b77e2bc/src/virtualenv/discovery/builtin.py#L107-L120

---

_Comment by @MichaReiser on 2024-02-19 13:04_

> We _do_ correctly find `python.exe` with #1381. 

I think #1381 only fixes the problem for `find_default_python` but not when calling `uv --python 3.8` where 3.8 is the default python installation

---

_Referenced in [astral-sh/uv#1693](../../astral-sh/uv/issues/1693.md) on 2024-02-19 15:00_

---

_Referenced in [astral-sh/uv#1711](../../astral-sh/uv/pulls/1711.md) on 2024-02-19 17:17_

---

_Closed by @MichaReiser on 2024-02-22 07:47_

---

_Referenced in [astral-sh/uv#2056](../../astral-sh/uv/issues/2056.md) on 2024-02-29 01:44_

---

_Referenced in [astral-sh/ty#684](../../astral-sh/ty/issues/684.md) on 2025-06-19 19:13_

---
