---
number: 14017
title: uv sync failed to bytecode-compile Python file because it failed to create temporary script file
type: issue
state: open
author: m-ad
labels:
  - needs-mre
assignees: []
created_at: 2025-06-13T09:38:21Z
updated_at: 2025-08-03T08:43:54Z
url: https://github.com/astral-sh/uv/issues/14017
synced_at: 2026-01-10T01:25:41Z
---

# uv sync failed to bytecode-compile Python file because it failed to create temporary script file

---

_Issue opened by @m-ad on 2025-06-13 09:38_

### Summary

`uv sync` and `uv run` do not work with bytecode compliation enabled (see example below for error message).


## Minimal working example
run in empty folder:
```cmd
❯ uv init && uv sync --compile-bytecode
Initialized project `test2`
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 1 package in 7ms
error: Failed to bytecode-compile Python file in: .venv\Lib\site-packages
  Caused by: Failed to create temporary script file
  Caused by: failed to write to file `C:\Users\mad\AppData\Local\uv\cache\.tmpTi173P\pip_compileall.py`: Der Vorgang ist bei einer Datei mit einem geöffneten Bereich, der einem Benutzer zugeordnet ist, nicht anwendbar. (os error 1224)
```
Error message Deepl-translated to English: "The process cannot be used for a file with an open area that is assigned to a user. (os error 1224)"

## Additional info
### no issues without bytecode-compilation
`uv sync` runs fine without the `--compile-bytecode` flag, also in bigger projects.

### uv run works fine
Also, I did not notice any issues when running real code with `uv run` (which should perform the bytecode compilation if I understand it correctly - however, I see no `*.pyc` files in `.venv\Lib\site-packages`, not sure where to look for them or how to confirm bytecode-compilation on the first run).

### reboot does not help
Because it looks like some kind of permission/lock issue, I rebooted my machine - no effect.

### Contents of cache folder:
```
❯ ls -l C:\Users\mad\AppData\Local\uv\cache\
total 537
-rw-r--r-- 1 MAD 1049089 43 Feb  7 16:51 CACHEDIR.TAG
drwxr-xr-x 1 MAD 1049089  0 Jun 13 09:22 archive-v0
drwxr-xr-x 1 MAD 1049089  0 Jun 13 09:22 builds-v0
drwxr-xr-x 1 MAD 1049089  0 Feb 12 14:40 environments-v1
drwxr-xr-x 1 MAD 1049089  0 Mar 19 16:28 environments-v2
drwxr-xr-x 1 MAD 1049089  0 Feb 12 14:58 git-v0
drwxr-xr-x 1 MAD 1049089  0 Apr  1 10:23 interpreter-v4
drwxr-xr-x 1 MAD 1049089  0 Feb 18 11:41 sdists-v7
drwxr-xr-x 1 MAD 1049089  0 Mar 28 14:03 sdists-v8
drwxr-xr-x 1 MAD 1049089  0 Jun 12 16:36 sdists-v9
drwxr-xr-x 1 MAD 1049089  0 Feb 12 14:40 simple-v15
drwxr-xr-x 1 MAD 1049089  0 May 27 11:26 simple-v16
drwxr-xr-x 1 MAD 1049089  0 Feb 18 11:40 wheels-v3
drwxr-xr-x 1 MAD 1049089  0 Feb 20 12:44 wheels-v4
drwxr-xr-x 1 MAD 1049089  0 Apr  1 10:24 wheels-v5
```
### Environment
- uv 0.7.12 (dc3fd4647 2025-06-06)
- Microsoft Windows 11 Enterprise (10.0.26100)

### Platform

Microsoft Windows 11 Enterprise (10.0.26100)

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

### Python version

_No response_

---

_Label `bug` added by @m-ad on 2025-06-13 09:38_

---

_Label `bug` removed by @konstin on 2025-06-13 10:38_

---

_Label `needs-mre` added by @konstin on 2025-06-13 10:38_

---

_Comment by @konstin on 2025-06-13 10:39_

Does the system set any specific policies? I haven't seen a system blocking creating temp files before, so I'm not sure how this happens or what the correct option for circumventing this is.

---

_Comment by @m-ad on 2025-06-13 15:06_

@konstin Any suggestion where and what to look for? It's a company computer, so I don't have full admin rights, there are group policies, Cyberreason,... could well be related to something like that, but I have no clue how to figure it out. Any suggestion would be welcome! That said, I only noticed an issue with `uv` so far and only in this highly specific case.

---

_Comment by @konstin on 2025-06-13 15:12_

uv here is creating a named temporary directory and a file in it so it can run a python interpreter with it, otherwise I unfortunately don't know where this would be from.

---

_Comment by @m-ad on 2025-06-16 07:46_

Other things i have tried now:

---

**Changing the python version** from 3.13 to e.g. 3.10 does not change the error message.

---

**Changing the cache directory** does not help:
```cmd
❯ set UV_CACHE_DIR=C:\Users\mad\Desktop\cache
❯ uv sync --compile-bytecode
Resolved 1 package in 7ms
error: Failed to bytecode-compile Python file in: .venv\Lib\site-packages
  Caused by: Failed to create temporary script file
  Caused by: failed to write to file `C:\Users\mad\Desktop\cache\.tmpA5ozuS\pip_compileall.py`: Der Vorgang ist bei einer Datei mit einem geöffneten Bereich, der einem Benutzer zugeordnet ist, nicht anwendbar. (os error 1224)
```
So its unrelated to anything (b)locking Windows' temp folder.

---

**Reproducing:**
* Could reproduce the problem at a different company machine (Windows 10 instead of Windows 11)
* On my private machine (Windows 10) this issue does *not* appear

**This points towards some Group Policy of Antivirus interfering** - still no clue how to go about this.

---

**Byte-compilation with [compileall](https://docs.python.org/3/library/compileall.html)** works fine for the freshly initialized virtual environment:
```cmd
❯ uv init && uv sync
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 1 package in 11ms
Audited in 0.02ms

❯ .venv\Scripts\activate.bat && python -m compileall .venv
Listing '.venv'...
Listing '.venv\\Lib'...
Listing '.venv\\Lib\\site-packages'...
Listing '.venv\\Scripts'...
Compiling '.venv\\Scripts\\activate_this.py'...
```
No error here. Also, directly afterwards, no error anymore on `uv sync --compile-bytecode`.
```cmd
❯ deactivate.bat

❯ uv sync --compile-bytecode
Resolved 1 package in 7ms
Bytecode compiled 1 file in 1.27s
```
However, this only works on the "empty" environment. If I add anything new:
```cmd
❯ uv add tqdm
Resolved 3 packages in 465ms
Installed 2 packages in 166ms
 + colorama==0.4.6
 + tqdm==4.67.1

❯ uv sync --compile-bytecode
Resolved 3 packages in 8ms
error: Failed to bytecode-compile Python file in: .venv\Lib\site-packages
  Caused by: Failed to create temporary script file
  Caused by: failed to write to file `C:\Users\mad\AppData\Local\uv\cache\.tmpWx282E\pip_compileall.py`: Der Vorgang ist bei einer Datei mit einem geöffneten Bereich, der einem Benutzer zugeordnet ist, nicht anwendbar. (os error 1224)
```

And going the activate-venv-and-compilall route a second time does not work here:

```cmd
❯ .venv\Scripts\activate.bat && python -m compileall .venv
Listing '.venv'...
Listing '.venv\\Lib'...
Listing '.venv\\Lib\\site-packages'...
Listing '.venv\\Lib\\site-packages\\colorama'...
Listing '.venv\\Lib\\site-packages\\colorama\\tests'...
Listing '.venv\\Lib\\site-packages\\colorama-0.4.6.dist-info'...
Listing '.venv\\Lib\\site-packages\\colorama-0.4.6.dist-info\\licenses'...
Listing '.venv\\Lib\\site-packages\\tqdm'...
Listing '.venv\\Lib\\site-packages\\tqdm\\contrib'...
Listing '.venv\\Lib\\site-packages\\tqdm-4.67.1.dist-info'...
Listing '.venv\\Scripts'...

❯ deactivate.bat

❯ uv sync --compile-bytecode
Resolved 3 packages in 8ms
error: Failed to bytecode-compile Python file in: .venv\Lib\site-packages
  Caused by: Failed to create temporary script file
  Caused by: failed to write to file `C:\Users\mad\AppData\Local\uv\cache\.tmp4MScUE\pip_compileall.py`: Der Vorgang ist bei einer Datei mit einem geöffneten Bereich, der einem Benutzer zugeordnet ist, nicht anwendbar. (os error 1224)
```
🤷

---

Forgot to do the obvious thing in the beginning and run `uv sync` with `-v`, so here is the output of that, although I don't see any obvious clue.
```cmd
❯ uv init && uv sync --compile-bytecode -v
Initialized project `test5`
DEBUG uv 0.7.12 (dc3fd4647 2025-06-06)
DEBUG Found project root: `C:\Users\mad\Desktop\tmp\test5`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\Users\mad\Desktop\tmp\test5`
DEBUG Reading Python requests from version file at `C:\Users\mad\Desktop\tmp\test5\.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.13 in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\mad\AppData\Roaming\uv\python`
DEBUG Found managed installation `cpython-3.13.2-windows-x86_64-none`
DEBUG Found `cpython-3.13.2-windows-x86_64-none` at `C:\Users\mad\AppData\Roaming\uv\python\cpython-3.13.2-windows-x86_64-none\python.exe` (managed installations)
Using CPython 3.13.2
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: C:\Users\mad\AppData\Roaming\uv\python\cpython-3.13.2-windows-x86_64-none\python.exe
DEBUG Released lock at `C:\Users\mad\AppData\Local\Temp\uv-e962231ff4ec6526.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test5 @ file:///C:/Users/mad/Desktop/tmp/test5
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.2
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: test5*
DEBUG Searching for a compatible version of test5 @ file:///C:/Users/mad/Desktop/tmp/test5 (*)
DEBUG Tried 1 versions: test5 1
DEBUG all marker environments resolution took 0.001s
Resolved 1 package in 12ms
DEBUG Using request timeout of 30s
DEBUG Starting 22 bytecode compilation workers
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Bytecode compilation worker exiting: Ok(())
DEBUG Released lock at `C:\Users\mad\Desktop\tmp\test5\.venv\.lock`
error: Failed to bytecode-compile Python file in: .venv\Lib\site-packages
  Caused by: Failed to create temporary script file
  Caused by: failed to write to file `C:\Users\mad\AppData\Local\uv\cache\.tmpfK3YSS\pip_compileall.py`: Der Vorgang ist bei einer Datei mit einem geöffneten Bereich, der einem Benutzer zugeordnet ist, nicht anwendbar. (os error 1224)
```



---

_Comment by @DimitriPapadopoulos on 2025-08-03 08:18_

From [System Error Codes (1000-1299)](https://learn.microsoft.com/en-us/windows/win32/debug/system-error-codes--1000-1299-):
> #### ERROR_USER_MAPPED_FILE
> 
> 1224 (0x4C8)
> 
> The requested operation cannot be performed on a file with a user-mapped section open.

At first sight, it looks like some other piece of software is interfering:
* Most probably antivirus software has identified the new temporary file as producing protentially executable code, and inspects it or [sends it back to the cloud](https://learn.microsoft.com/en-us/defender-endpoint/cloud-protection-microsoft-antivirus-sample-submission?view=o365-worldwide) for further inspection.
* Windows [SystemProtection](https://support.microsoft.com/en-us/windows/system-protection-e9126e6e-fa64-4f5f-874d-9db90e57645a) might be kicking in if it's enabled,
* Backup or cloud synchronisation programs (like Dropbox) are usually not set up to look into temporary files.

Why not ask some AI for guidance on how to detect the rogue software?
<details>
<summary>Sample AI suggestions</summary>

> 
> The `ERROR_USER_MAPPED_FILE` (Windows error code **1224**) indicates that a file cannot be accessed because it is **currently mapped into memory**—typically by another process using `CreateFileMapping()` and `MapViewOfFile()`. This can block certain operations such as deletion, renaming, or modifying the file.
> 
> ### 🔍 To diagnose which software is interfering:
> 
> #### 1. Use Sysinternals Process Explorer (or Handle.exe)
> 
> Microsoft Sysinternals provides tools that help detect file handles and memory-mapped files:
> 
> #### 👉 Method A: Using Process Explorer
> 
> 1. Download Process Explorer:
>     https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer
> 2. Run it **as Administrator**.
> 3. Press **Ctrl + F** (Find Handle or DLL).
> 4. Enter the filename or path of the problematic file.
> 5. Look for entries that mention:
>    * `File Mapping`
>    * `MapViewOfFile`
>    * The software that has a handle to the file
> 
> This will help you identify the PID and process name using or locking the file.
> 
> #### 👉 Method B: Using Handle.exe
>  1. Download Handle from Sysinternals:
>    https://learn.microsoft.com/en-us/sysinternals/downloads/handle
> 2. Open Command Prompt **as Administrator**.
> 3. Run:
>    ```
>    handle.exe filename
>    ```
>    or
>    ```
>    handle.exe > handles.txt && notepad handles.txt
>    ```
>    to search more easily.
> 
> It shows which process has the file **mapped or opened**.
> 
> #### 2. Use Resource Monitor
> 
> 1. Press `Win + R`, type `resmon`, and hit Enter.
> 2. Go to the CPU tab.
> 3. Expand **Associated Handles**.
> 4. Enter part of the file path to filter.
> 5. See which process is holding a handle to the file.
</details>

Unfortunately, AI guidance may not easily apply to temporary, transient files.

Could you ask for assistance from IT? They have most probably installed/configured the software that interferes with uv. Are they knowledgeable enough to clean up their own mess?

---

_Comment by @DimitriPapadopoulos on 2025-08-03 08:33_

The question remains, how to defend against rogue antivirus software inside uv itself?

I understand uv is unable to create `.tmpfK3YSS\pip_compileall.py`.
* Is uv trying to repeatedly (re)create the same `pip_compileall.py` file inside the same temporary directory (`.tmpfK3YSS` in this case)?
* Has uv previously created a different file inside the same temporary directory `.tmpfK3YSS`, perhaps deciding the antivirus software to inspect/block the whole directory?

In both cases, it might help to use different temporary directories at each bytecode compilation step, if feasible at all. Or keep retrying until reaching a timeout.

---

_Referenced in [invoke-ai/InvokeAI#7686](../../invoke-ai/InvokeAI/issues/7686.md) on 2025-08-03 08:49_

---
