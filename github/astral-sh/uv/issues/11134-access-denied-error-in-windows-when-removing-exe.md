---
number: 11134
title: "Access Denied error in Windows when removing .exe files from `.venv`"
type: issue
state: open
author: nathanjmcdougall
labels:
  - windows
  - needs-decision
assignees: []
created_at: 2025-01-31T14:19:45Z
updated_at: 2025-11-27T09:54:09Z
url: https://github.com/astral-sh/uv/issues/11134
synced_at: 2026-01-07T13:12:18-06:00
---

# Access Denied error in Windows when removing .exe files from `.venv`

---

_Issue opened by @nathanjmcdougall on 2025-01-31 14:19_

### Summary

### Example Error Message

```Powershell
PS C:\Users\namc\Temp\permissionsissue> uv remove ruff
Resolved 1 package in 8ms
error: failed to remove file `C:\Users\namc\Temp\permissionsissue\.venv\Lib\site-packages\..\..\Scripts\ruff.exe`: Access is denied. (os error 5)
```

### Overview of the issue

On Windows, a `uv` managed environment will try to delete `.exe` files in `.venv/Scripts`. However, these might be running via some external process, creating a permissions error, and preventing deletion.

This is especially an issue with the `uv remove` command. First, the lockfile is updated, then uv will attempt to delete `.exe` files and encounter an error. This would leave the `.venv` and lockfile out-of-sync. This means all subsequent calls involving syncing will give a permissions error too, e.g. `uv sync`, `uv add numpy`, etc. From the user's perspective they are somewhat stuck.

### A Specific Example

Trying to remove Ruff with `uv` can lead to a permissions issue when using the [VS Code Ruff extension](https://github.com/astral-sh/ruff-vscode/issues) on Windows, since uv is trying to delete `.venv/Scripts/ruff.exe` while the Ruff Language Server is still using it. I suspect this is the root cause of #7382.

### Compared behaviour with `pip`

The same permissions error occurs when running `uv pip uninstall ruff` rather than `uv remove`. However, pip itself can cope with this situation: `pip uninstall ruff` will move the executable into a temporary directory. It warns the user that it can be deleted manually, giving the temp dir path:
```
WARNING: Failed to remove contents in a temporary directory 'C:\Users\namc\AppData\Local\Temp\pip-uninstall-lvja8bs4'.
You can safely remove it manually.
```

I reckon this is a great solution - it keeps the `.venv` properly synced while still letting the process continue.

In the specific case I mentioned before, the Language Server can keep running (or perhaps the VS Code extension identify that the exe has moved and restart it in an isolated environment), but in any case, the `uv remove ruff` command would run successfully.

### Steps to reproduce:

0. Ensure VS Code is installed with the `charliermarsh.ruff` extension.
1. Open an empty directory in VS Code, and run the following commands from Powershell within VS Code:
2. `uv init`
3. `uv add ruff`
4. In VS Code: View > Command Palette > Developer: Reload Window
5. `code hello.py`
6. In VS Code: View > Output > Ruff Language Server. Wait until the first `INFO` message appears to show the server has started.
7. `uv remove ruff` should reproduce the error.

Whereas at step 7, there is a workaround via pip which only gives a warning and not an error:

```
uv add --dev pip
uv run pip uninstall ruff
uv remove ruff pip
```

I'm sorry if this is a bit clunky, unfortunately I tried to reproduce this without VS Code but it's beyond my expertise.

#### Notes

The point of step 5 is to open a Python file, which seems to help to trigger the Ruff language server to start.
While the extensions start up after the reload in step 4, it may take a minute for the Ruff language server to be available on the Output menu.

An example message for step 6 is;

```
INFO main ruff_server::session::index: Registering workspace: c:\Users\namc\Temp\permissionsissue
```

### VS Code Version information

Version: 1.96.4 (user setup)
Commit: cd4ee3b1c348a13bafd8f9ad8060705f6d4b9cba
Date: 2025-01-16T00:16:19.038Z
Electron: 32.2.6
ElectronBuildId: 10629634
Chromium: 128.0.6613.186
Node.js: 20.18.1
V8: 12.8.374.38-electron.0
OS: Windows_NT x64 10.0.19045

|              |                      |
|--------------|----------------------|
| Identifier   | `charliermarsh.ruff`   |
| Version      | 2025.4.0             |
| Last Updated | 2025-01-28, 10:14:12 |



### Platform

Windows 10 x86-64

### Version

uv 0.5.21 (3478c068b 2025-01-17)

### Python version

Python 3.11.6

---

_Label `bug` added by @nathanjmcdougall on 2025-01-31 14:19_

---

_Label `bug` removed by @charliermarsh on 2025-01-31 15:01_

---

_Label `windows` added by @charliermarsh on 2025-01-31 15:01_

---

_Label `needs-decision` added by @charliermarsh on 2025-01-31 15:01_

---

_Comment by @charliermarsh on 2025-01-31 15:05_

Thank you for the clear issue! I'm a little undecided on the complexity tradeoffs here (as in: it seems fairly complex to support this) -- am interested in getting other opinions on it.

---

_Comment by @FishAlchemist on 2025-01-31 16:22_

This problem also occurs in the ``uv tool``, as I mentioned in my previous issue.
* https://github.com/astral-sh/uv/issues/8528

@charliermarsh If you don't want to move the executable file, have you considered trying to delete it before changing the locked file. In simple terms, it means to undo the changes that failed.

---

_Comment by @charliermarsh on 2025-01-31 18:02_

On reflection, I think it's probably worth doing this for executables.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-31 18:37_

---

_Comment by @charliermarsh on 2025-01-31 21:46_

Annoyingly, I can't get this to reproduce.

---

_Comment by @charliermarsh on 2025-01-31 21:50_

I'm confused by it. Windows is definitely letting me uninstall `ruff.exe` while the language server is running.

---

_Comment by @nathanjmcdougall on 2025-01-31 23:17_

One possibility is that VS Code settings may affecting whether the ruff extension is using the ruff executable in the project venv, versus elsewhere. I wonder whether it is related to whether VS Code has identified the project .venv yet? Perhaps after running `uv init`, try View > Command Palette > Python: Select Interpreter > `.\.venv\Scripts\python.exe`, and then when running the Reload window step the extension will be aware of where to find the ruff executable.

It definitely seemed like `uv remove ruff` was successful before the language server started, but perhaps that was just a coincidence, and it was really something else. I only have circumstantial evidence the language server is the culprit. The ruff extension is definitely related though, since the issue stops reproducing when the extension is disabled.

---

_Comment by @nathanjmcdougall on 2025-01-31 23:34_

Another bit of info that might help. I am writing software which subprocesses `uv`. As a part of the test suite, I create uv projects in temporary directories managed by pytest. I've noticed that my VS Code process will somehow become aware of these projects and start locking their `ruff.exe` files too. This is fairly intermittent. I'm not sure exactly what the extension is doing but it seems to have some kind of sentinel which is looking for Ruff executables. Disabling the Ruff extension stops the issue.

Here's an example of the sort of error I get, where `uv` is working away in a temp directory but somehow the ruff.exe gets locked:
```
usethis._integrations.uv.errors.UVDepGroupError: Failed to remove 'ruff' from the 'dev' dependency group:
E               Failed to run uv subprocess:
E               stderr='error: failed to remove file `C:\\Users\\namc\\AppData\\Local\\Temp\\pytest-of-namc\\pytest-5015\\test_remove_NetworkConn_ONLINE0\\.venv\\Lib\\site-packages\\..\\..\\Scripts\\ruff.exe`: Access is denied. (os error 5)\n'
```


---

_Comment by @tim-stephenson on 2025-02-14 17:51_


I think my problem is related (and hopefully easy to reproduce). On Windows, when uv fails to uninstall a python package due to that executable (whether that be an `exe`, `dll`, or `pyd`) being currently running in a separate process, it will leave the `site-packages` in a corrupt state which requires "manually" deleting the partially deleted package.


## How to reproduce 

- Have uv installed and be in some virtual environment with python
- `uv pip install polars` (using polars as an example, almost any python C/C++/Rust/Fortran extension module or executable should demonstrate this)
- In a separate terminal
  - get into the same virtual environment
  - start a python session by typing `python` 
  - `import polars as pl`
- Back in the first terminal
  - `uv pip uninstall polars` 
    - Should get a os error about lack of permission to remove the executable (os error 5)
  - `uv pip install polars`
    - Should get `The system cannot find the file specified. (os error 2)` on `Lib\site-packages\polars-x.x.x.dist-info\METADATA`
- Back in the separate terminal
  - type `exit()` to close this python session
- Back in the first termnial
  - neither `uv pip install polars` or `uv pip uninstall polars` works    


The resulting conclusion is a messed-up state that requires the user to dig into `site-packages` and remove files.

## Why telling users to "just don't do this" is not viable

- Easy to forget you forgot to close a python session and corrupt your site packages
- This can occur when not explicitly calling `uv pip uninstall` as `uv pip install my_fake_package` will first uninstall packages for which a different version is needed to comply with the dependencies of `my_fake_package`


## Solution

- Make uninstalling a atomic operation, it either entirely happens or fails

---

_Comment by @andyvandy on 2025-06-02 13:58_

I noticed that this issue ceased when I upgraded to win 11 from win 10. 

The solution here may just be to upgrade to win 11?

<details>
  <summary>Extra Ramblings</summary>
<p>I haven't been able to find an official doc but it seems win 11 now no longer locks dlls and exes which are loaded into memory. </p>


I had been plagued by these issues for years and I used to scan for processes and their loaded dlls in order to kill them so that I could upgrade tools. It was quite a pain so I was glad to delete that code recently. 
  
</details>




---

_Comment by @slingshotsys on 2025-06-02 18:32_

> The solution here may just be to upgrade to win 11?

I also experience this error on Windows 11 Pro 23H2, specifically when VS Code is running.



---

_Comment by @matthuisman on 2025-06-02 23:17_

I hit the same issue as @tim-stephenson with charset-normalizer (used by requests).

Oddly, it leads do another issue with dist-info for it is empty.
I can then install a newer version and end up with uv pip list showing both versions with no way to uninstall the corrupt version :(

pip list doesnt have the same issue. it must check for empty dist-info and ignore them?


---

_Referenced in [astral-sh/uv#13836](../../astral-sh/uv/issues/13836.md) on 2025-06-04 14:06_

---

_Comment by @momostein on 2025-06-04 15:38_

As per my duplicate issue #13836, I've been having a relevant issue with the same root cause.

As a workaround, I've edited the [`link-mode`](https://docs.astral.sh/uv/reference/settings/#link-mode) setting in my global `%AppData%/uv/uv.toml` config file. And now I've tried all possibilities.

## These are my findings

### `hardlink` (default)
 
- Running `uv run ruff server` locks all `ruff.exe` files in all uv virtual environments
- While the server is running, ruff can not be removed/updated in any project on the same file system.
- Interactions with `uv cache clean`
  - `uv run ruff server` still works after `uv cache clean`
  - can even run `uv cache clean` while ruff server is running

### `copy`

- Running `uv run ruff server` only locks `ruff.exe` files in the current venv
- Can update/remove ruff in other venvs, not in the current venv
- Interactions with `uv cache clean`
  - `uv run ruff server` still works after `uv cache clean`
  - I can even run `uv cache clean` while ruff server is running   
- Obviously less efficient in both time and space

### `symlink`
 
- Running `uv run ruff server` does not lock any ruff.exe inside any virtual environments
- Can even delete the active `.venv/Scripts/ruff.exe` while it is running.
- Interactions with `uv cache clean`
 - `uv run ruff server` does not work after `uv cache clean`
 - `uv cache clean` fails while `uv run ruff server` is running
- `uv cache prune`
  - `uv run ruff server` still works after `uv cache prune`

### `clone`

Doesn't seem to work on windows, falls back to full copy.

## My current setting

I'll be using the `symlink` link-mode for now. At least until I encounter any other bugs. I'll take the caveat that my virtual environments will break if I run `uv cache clean`. I can easily delete my .venvs and then quickly regenerate them anyway thanks to the amazing speed of uv. `uv cache prune` should usually be enough.

I do have a different opinion on my `uv tool` installations though, I think that copy might be the better option there. Reinstalling them might be a bigger hassle if they ever break due to cache cleaning. There I think that `copy` might be the better link mode.

As for now, there seems to be no individual link-mode configuration setting for `uv tool` except for the `--link-mode` cli option.


**EDIT:**
A ruff tool installation with: `uv tool install ruff --link-mode=symlink`, does not break after running `uv cache clean`. I have no clue how this is supposed to work then...

So yeah, `link-mode = "symlink"` will probably fix this issue, with the caveat that virtual environments will break after running `uv cache clean`.

I would love to see some windows file system expert's opinion on this because I only have a surface level understanding on these hard links and symbolic links.

---

_Referenced in [astral-sh/uv#13986](../../astral-sh/uv/issues/13986.md) on 2025-06-17 19:57_

---

_Comment by @drmikehenry on 2025-07-17 23:45_

I recently learned of another scenario where using hardlinks on Windows causes problems.  When running two simultaneous CI jobs with Gitlab, frequently one job was unable to fully cleanup the `.venv` directory because the other was actively using the other virtual environment.  The venvs are coupled together because both have their packages hardlinked from the same cache area.  Using `UV_LINK_MODE=copy` decouples these jobs, allowing them to complete successfully.

Since a primary reason for using virtual environments is isolation, this accidental coupling between venvs seems problematic for a default.  The developer who brought this to my attention spent many hours trying to diagnose the cause of these sporadic CI failures that weren't reproducible on the developer's own machine because the jobs weren't run simultaneously.

In case it's helpful, below is a method of demonstrating the coupling between the `.venv` trees.

- In one `cmd.exe` prompt, create a virtual environment with `rich` installed, then hold open a file from that package:

    mkdir v1
    cd v1
    uv venv
    uv pip install rich
    more .venv\lib\site-packages\rich\__init__.py

- In a second `cmd.exe` prompt, create a second venv with `rich` and try to destroy it:

    mkdir v2
    cd v2
    uv venv
    uv pip install rich
    rd /s /q .venv

The second .venv can't be removed; the error is:

```
.venv\Lib\SITE-P+1\rich\__init__.py - The process cannot access the file because it is being used by another process.
```

You can use the following command to see the hard links
involved:

```
fsutil hardlink list .venv\lib\site-packages\rich\__init__.py
```

Resulting in:

```
\tmp\v1\.venv\Lib\site-packages\rich\__init__.py
\tmp\v2\.venv\Lib\site-packages\rich\__init__.py
\Users\mike\AppData\Local\uv\cache\archive-v0\-7ANQ3EWvBhrZlrAfr6Wy\rich\__init__.py
```


---

_Referenced in [astral-sh/uv#15108](../../astral-sh/uv/issues/15108.md) on 2025-11-26 18:40_

---

_Comment by @nathanjmcdougall on 2025-11-26 18:43_

Up to this point, my team and I at work have had a good experience with using symlink mode on Windows as a workaround to this issue (despite its use being discouraged in the docs).

However, recently an issue I've encountered is that the 260-character path length limit on Windows can cause problems. For example, the latest version of `ipywidgets` hits this:

```
error: Failed to install: jupyterlab_widgets-3.0.16-py3-none-any.whl (jupyterlab-widgets==3.0.16)                                                                                   
  Caused by: failed to symlink file from C:\Users\namc\AppData\Local\uv\cache\archive-v0\9Qq4SbCpOd7-qk7lGBlCR\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js to C:\Users\namc\Repositories\demo\.venv\Lib\site-packages\jupyterlab_widgets-3.0.16.data\data\share\jupyter\labextensions\@jupyter-widgets\jupyterlab-manager\static\packages_base_lib_index_js-webpack_sharing_consume_default_jquery_jquery.5dd13f8e980fa3c50bfe.js: A required privilege is not held by the client. (os error 1314)
```

---

_Comment by @konstin on 2025-11-27 09:54_

@nathanjmcdougall Can you open a separate issue for that?

---

_Referenced in [astral-sh/uv#16877](../../astral-sh/uv/issues/16877.md) on 2025-11-27 19:40_

---
