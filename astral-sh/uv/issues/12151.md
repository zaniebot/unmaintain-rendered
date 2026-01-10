```yaml
number: 12151
title: python run by uv is not the real python process
type: issue
state: closed
author: fabioz
labels:
  - question
assignees: []
created_at: 2025-03-13T12:55:43Z
updated_at: 2025-10-19T15:59:35Z
url: https://github.com/astral-sh/uv/issues/12151
synced_at: 2026-01-10T03:23:53Z
```

# python run by uv is not the real python process

---

_Issue opened by @fabioz on 2025-03-13 12:55_

### Summary

As far as I understand, this is by design, but it still gives me headaches, so, reporting as bug (feel free to move to feature request or close if it will never be changed).

i.e.: in some code as:

```
def main():
    import os
    import sys

    print(sys.executable)
    if len(sys.argv) > 1 and sys.argv[1] == "check subprocess":
        print("In subprocess", sys.executable, "pid", os.getpid())

    else:
        import subprocess

        opened_process = subprocess.Popen(
            [sys.executable, __file__, "check subprocess"]
        )
        print("Current process", os.getpid())
        print("Opened process", opened_process.pid)


if __name__ == "__main__":
    main()
```

the output is actually:

```
<path>\.venv\Scripts\python.exe
Current process 8188
Opened process 26060
<path>\.venv\Scripts\python.exe
In subprocess <path>\.venv\Scripts\python.exe pid 25536
```

So, there are 3 different processes in play! Seeing in the process explorer it actually shows `<user>\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\python.exe` instead of the python from uv, which is pretty misleading...

Note: I found this because tests were failing and I had no idea why the pid written to a lock file wasn't matching the pid of the process I just launched.

Isn't it possible for `uv run python ...` to launch the final executable instead of a launcher which then launches the final executable?... it seems that `uv run` could build the whole env and run the final python directly instead of that shim (and then even terminating that call would do the right thing instead of having to always terminate the whole tree).

### Platform

Windows 11 x86_64

### Version

uv 0.6.4 (04db70662 2025-03-03)

### Python version

Python 3.11.11

---

_Label `bug` added by @fabioz on 2025-03-13 12:55_

---

_Comment by @FishAlchemist on 2025-03-16 13:47_

After doing some research, I found that you're actually right. It is indeed Python calling Python. The ``.venv\Scripts\python.exe``  reads the ``home`` from ``pyvenv.cfg``  and then uses it to call Python. So, the final program that executes your code is ``<user>\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\python.exe``, not ``<path>\.venv\Scripts\python.exe``

Call Graph by ``subprocess.popen``:
``<path>\.venv\Scripts\python.exe`` (The PID obtained by subprocess.Popen)
    └── ``<user>\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\python.exe`` (The PID obtained from a subprocess executing code)

By the way, it seems that the standard built-in venv is like that too.


---

_Comment by @zanieb on 2025-03-16 14:05_

I could see how this would be confusing, but I think this is the same as if you activated the environment and ran `python.exe`? I don't think we can differ from the "standard" way of invoking Python here.

---

_Label `bug` removed by @zanieb on 2025-03-16 14:05_

---

_Label `question` added by @zanieb on 2025-03-16 14:05_

---

_Comment by @zanieb on 2025-03-16 14:06_

The `python` executables in virtual environments are just "launchers"

ref https://github.com/python/cpython/blob/d457345bbc6414db0443819290b04a9a4333313d/Lib/venv/__init__.py#L261-L267

---

_Comment by @fabioz on 2025-03-16 15:22_

> I don't think we can differ from the "standard" way of invoking Python here.

Why not? i.e.: What I'm used to use is conda, where python gives me the real python (I find that a better experience than double launching which actually is responsible for issues such as killing but leaving orphaned process, pid reference which is not the real pid).

---

_Comment by @notatallshaw on 2025-03-16 16:58_

Some background, Conda virtual environments are not Python virtual environments: https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html#virtual-environments

Conda environments act more like user Python installations, e.g, they accept user site packages by default, and they don't conform to standards that virtual environments match, also they do a lot more control over env variables and binaries available. This leads to pros and cons of such an environment, but scripts and applications can be broken by this kind of change (e.g. I’ve had to do a lot of support work in the past of applications that are broken by having the conda OpenSSL on that path in an activated conda environment).

I don't have a strong understand of the spec here, if this actually is part of the spec or venvs implementation, but I would caution the uv team from creating some alternative that deviates from the spec until they are confident they know what they want to do and support, as there is endless edge case behavior on stuff like this.




---

_Comment by @konstin on 2025-10-19 12:27_

It's important that `sys.executable` points to the venv Python to ensure that subprocesses start with the same Python environment. If we were to launch the base Python executable and someone were to launch a Python child process, that process wouldn't have the packages available in the current process. An example where this is important in PEP 517.

---

_Closed by @zanieb on 2025-10-19 15:59_

---
