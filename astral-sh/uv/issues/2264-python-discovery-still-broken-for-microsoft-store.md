```yaml
number: 2264
title: Python discovery still broken for Microsoft Store
type: issue
state: closed
author: NMertsch
labels:
  - bug
  - windows
assignees: []
created_at: 2024-03-07T06:24:46Z
updated_at: 2024-03-07T22:30:08Z
url: https://github.com/astral-sh/uv/issues/2264
synced_at: 2026-01-12T15:58:36Z
```

# Python discovery still broken for Microsoft Store

---

_@NMertsch_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I followed #2105 (fixed in 0.1.14) and #2208 (fixed in 0.1.15), and still cannot make uv discover my Python executables installed via the Microsoft Store.

With 0.1.13 and 0.1.14, I had the error message mentioned in the previous tickets (`failed to canonicalize path 'C:\Users\...\AppData\Local\Microsoft\WindowsApps\python3.11.exe'`).

With 0.1.15, I get this: `No Python 3.11 found through 'py --list-paths' or in 'PATH'. Is Python 3.11 installed?`

Details:
```text
PS C:\Users\nme> irm https://astral.sh/uv/install.ps1 | iex
Downloading uv 0.1.15 (x86_64-pc-windows-msvc)
Installing to C:\Users\nme\.cargo\bin
  uv.exe
Everything's installed!

PS C:\Users\nme> uv --version
uv 0.1.15 (a8ac7b1eb 2024-03-05)

PS C:\Users\nme> uv venv --verbose --python 3.11
 uv_interpreter::python_query::find_requested_python request=3.11
      0.008891s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python @ `3.11`
   uv_interpreter::python_query::windows::py_list_paths
      0.016581s   7ms DEBUG uv_interpreter::python_query `py` is not installed
  × No Python 3.11 found through `py --list-paths` or in `PATH`. Is Python 3.11 installed?

PS C:\Users\nme> py --list-paths
py: The term 'py' is not recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.

PS C:\Users\nme> python -V
Python 3.10.11
```

The relevant error message seems to be "No Python 3.11 found through `py --list-paths` or in `PATH`":
- `py --list-paths` does not work (`py` is unknown)
- `python` on my PATH is version 3.10

At least on my two Windows machines, the Microsoft Store installs all Python versions as `python3.X`, but I don't know how it determines which version `python` is (I always use `python3.X` explicitly):

```text
PS C:\Users\nme> python3.7 -V
Python 3.7.9
PS C:\Users\nme> where.exe python3.7
C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python3.7.exe

PS C:\Users\nme> python3.8 -V
Python 3.8.10
PS C:\Users\nme> where.exe python3.8
C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python3.8.exe

PS C:\Users\nme> python3.9 -V
Python 3.9.13
PS C:\Users\nme> where.exe python3.9
C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python3.9.exe

PS C:\Users\nme> python3.10 -V
Python 3.10.11
PS C:\Users\nme> where.exe python3.10
C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python3.10.exe

PS C:\Users\nme> python3.11 -V
Python 3.11.8
PS C:\Users\nme> where.exe python3.11
C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python3.11.exe
```

Interestingly, `uv` also fails to discover `3.10`, even though the `python` in my PATH is `3.10`:

```text
PS C:\Users\nme> uv venv --verbose --python 3.10
 uv_interpreter::python_query::find_requested_python request=3.10
      0.051711s   0ms DEBUG uv_interpreter::python_query Starting interpreter discovery for Python @ `3.10`
   uv_interpreter::python_query::windows::py_list_paths
      0.056948s   5ms DEBUG uv_interpreter::python_query `py` is not installed
  × No Python 3.10 found through `py --list-paths` or in `PATH`. Is Python 3.10 installed?

PS C:\Users\nme> python -V
Python 3.10.11

PS C:\Users\nme> where.exe python
C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python.exe
```

(Thanks everyone working on ruff and uv btw! <3)

---

_Comment by @charliermarsh on 2024-03-07 14:08_

Hey thanks for the clear write-up! I'll look into this again. It may be hard for me to debug because it is working as expected on my own Windows machine with the Windows Store Python, but I'll try some more of these cases.

---

_Label `windows` added by @charliermarsh on 2024-03-07 14:08_

---

_Comment by @charliermarsh on 2024-03-07 14:09_

In the meantime, don't hate me but if you're blocked, I might suggest installing Python from the python.org installers? It should give you a more reliable experience (at least with uv, though other tools like virtualenv also include special-casing for Windows Store).

---

_Assigned to @konstin by @charliermarsh on 2024-03-07 15:50_

---

_Comment by @notatallshaw on 2024-03-07 16:03_

Pinging @zooba, apologies if inappropriate.

But if you have a moment, is there a canonical way to determine the Microsoft Store Python installs available?

---

_Comment by @charliermarsh on 2024-03-07 16:17_

I think the issue is that we ignore the Windows Store paths (like `C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python3.7.exe`) when looking at `$PATH`, and instead rely on `py --list-paths` to give us the "real" interpreters, which instead look like `C:\Users\nme\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`.

---

_Comment by @charliermarsh on 2024-03-07 16:18_

I'm using "real" in quotes because I don't really understand how the Windows Store install is structured -- but I'm generally not sure how to go from `C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python3.9.exe` to the interpreter path above.

---

_Comment by @charliermarsh on 2024-03-07 16:26_

Maybe we just shouldn't be "ignoring" those, and should instead avoid trying to canonicalize them (which is where we see failures).

---

_Comment by @charliermarsh on 2024-03-07 16:35_

I think the intent of the filtering is to filter out the "App Execution aliases", there's some commentary that it may open up the Windows Store in some cases? I need to figure out how to reproduce this.

---

_Comment by @charliermarsh on 2024-03-07 16:39_

Ahhh I see it. I needed to check "App Installer" in "App execution aliases", I think.

---

_Comment by @charliermarsh on 2024-03-07 16:44_

So we need a way to reliably detect and filter out the "App Installer" redirects.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-07 16:44_

---

_Unassigned @konstin by @charliermarsh on 2024-03-07 16:44_

---

_Comment by @NMertsch on 2024-03-07 16:52_

> In the meantime, don't hate me but if you're blocked, I might suggest installing Python from the python.org installers?

I'm not blocked, but thank for once again for the amazing user experience (with ruff, uv, and GitHub Issues 😅)! It's highly appreciated.

If I can help with anything (e.g. run some commands, test pre-release builds), please let me know.

---

_Comment by @charliermarsh on 2024-03-07 16:53_

I'm now able to reproduce on my machine, I think!

---

_Label `bug` added by @charliermarsh on 2024-03-07 16:59_

---

_Comment by @charliermarsh on 2024-03-07 17:58_

The main issue is I can't figure out a reliable way to determine if the `python.exe` or `python3.exe` is a redirect to the Windows Store. `CreateFileW` is returning `INVALID_HANDLE_VALUE` for one of the two redirectors when trying to read the reparse point.

---

_Closed by @charliermarsh on 2024-03-07 20:47_

---

_Comment by @zooba on 2024-03-07 22:27_

> I think the issue is that we ignore the Windows Store paths (like `C:\Users\nme\AppData\Local\Microsoft\WindowsApps\python3.7.exe`) when looking at `$PATH`, and instead rely on `py --list-paths` to give us the "real" interpreters, which instead look like `C:\Users\nme\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`.

So the difference here is that the shorter path only exists when the user has configured it to be a global alias, while the latter will always be present whenever the app is installed. You could search ``C:\Users\nme\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.*`, since nobody else can use that publisher name, or code in the full package name. The hash at the end is stable, however if we ever release the standalone MSIX installers then the hash will be different for those (it's essentially based on the code signing certificate used, which doesn't change for the Store, but would change every few years for the standalone one).

---

_Comment by @zooba on 2024-03-07 22:30_

Also, if you're only looking at the longer paths, you'll never find the redirector stub. However, if you've found it from a PATH search and want to check, the way we do it in py.exe is in these two sections:

https://github.com/python/cpython/blob/5d0cdfe519e6f35ccae1a1adca1ffd7fac10cee0/PC/launcher2.c#L576-L588

https://github.com/python/cpython/blob/5d0cdfe519e6f35ccae1a1adca1ffd7fac10cee0/PC/launcher2.c#L781-L827

(Uses undocumented stuff, but should be stable enough. If anything here changes, then realistically the launcher is going to be significantly changed and probably more stuff needs to change as well.)

---
