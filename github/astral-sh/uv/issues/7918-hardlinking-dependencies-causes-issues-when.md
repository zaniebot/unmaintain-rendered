---
number: 7918
title: "hardlinking dependencies causes issues when trying to delete a `.venv` via python on Windows"
type: issue
state: open
author: linde12
labels:
  - bug
  - windows
  - cache
assignees: []
created_at: 2024-10-04T09:47:39Z
updated_at: 2025-12-03T00:56:24Z
url: https://github.com/astral-sh/uv/issues/7918
synced_at: 2026-01-07T13:12:17-06:00
---

# hardlinking dependencies causes issues when trying to delete a `.venv` via python on Windows

---

_Issue opened by @linde12 on 2024-10-04 09:47_

## Scenario
I have a script in a venv (EnvA) that:
- creates a new uv venv (EnvB)
- installs some packages to the new venv (EnvB)
- runs a script in that venv(EnvB)
- tries to delete EnvB via the EnvA script.

However the venv deletion fails on Windows since there are overlapping dependencies between the two environments (both depend on e.g. `markupsafe` which contains `pyd` files that are automatically loaded by python)

This is because UV uses hardlinks to the uv cache, so deleting that file fails since it is hardlinked to the file in uv cache and that file (.pyd) is automatically loaded by python.

Switching to using symlinks solves the issue, but i don't have control over how users will run this script. What is the decision behind using hardlinks by default on windows? I have little control over how the application runs `uv` (done by 3rd party library) so this is causing issues for my users. I guess i could patch the environment variable `UV_LINK_MODE` env var before invoking the 3rd party lib, but that feels like a hack and i'm not sure if it will work well.


## Minimally reproducible example
1. Create a new venv: `uv venv .venv`
2. Install `markupsafe` into that venv: `uv pip install markupsafe`
3. Create a `main.py` with contents like so:
```python
import subprocess
import shutil
import os

os.mkdir("testdir")
subprocess.run(["uv", "venv", ".venv"], cwd="./testdir")
subprocess.run(["uv", "pip", "install", "markupsafe"], cwd="./testdir")
shutil.rmtree("testdir")
```
4. Run `main.py` via `python main.py`
5. Notice that it will fail on Windows: `PermissionError: [WinError 5] Access is denied: 'testdir\\.venv\\Lib\\site-packages\\markupsafe\\_speedups.cp310-win_amd64.pyd'`
6. Try changing link mode (e.g. via environment variable) to symlink and see that it then works when running `python main.py`

I think it boils down to python automatically loading `.pyd` files in your venv and then uv hardlinking to the cache, and when the file is loaded/has open file descriptors windows will not allow us to delete the hardlink. This is not only an issue for only markupsafe, but all libraries that use `.pyd` files, this was just an example.

## tl;dr
if you have a python script, with any dependency containing a `.pyd` file, and this script tries to delete a `.venv` containing a hardlink to the same `.pyd` file then Windows will throw you an "Access is denied" error.

## Version
uv: `0.4.17`

---

_Comment by @linde12 on 2024-10-04 13:06_

> I guess i could patch the environment variable UV_LINK_MODE env var before invoking the 3rd party lib, but that feels like a hack and i'm not sure if it will work well.

symlink mode won't work either (unless developer mode or admin) but copy can be a workaround i guess :-/

---

_Label `bug` added by @zanieb on 2024-10-04 13:11_

---

_Label `windows` added by @zanieb on 2024-10-04 13:11_

---

_Label `cache` added by @zanieb on 2024-10-04 13:11_

---

_Comment by @charliermarsh on 2024-10-04 13:12_

I appreciate the clear write-up but I'm not sure that this is a bug and I'm also not sure that there's anything for us to do here. I guess we could "always" copy `.pyd` files on Windows, even if hardlinks are enabled?

---

_Comment by @zanieb on 2024-10-04 13:21_

It seems weird that you can't delete any hardlink when an fd is open? It seems like they should just stop you from deleting the last one.

---

_Comment by @linde12 on 2024-10-04 13:50_

> I appreciate the clear write-up but I'm not sure that this is a bug and I'm also not sure that there's anything for us to do here. I guess we could "always" copy `.pyd` files on Windows, even if hardlinks are enabled?

Thanks. Yeah i'm not sure either, just wanted to bring it up. It's a foot-shotgun turned rabbit hole that i encountered. 

Copying would solve the problem, at the cost of disk space and performance (?) I'm not sure if that's better in favor of being more compatible with how it works using pip/virtualenv, or if this can be considered an acceptable discrepancy? Maybe it's fine if it is documented somehow, but its also kind of very specific.


> It seems weird that you can't delete any hardlink when an fd is open? It seems like they should just stop you from deleting the last one.

Right?! I was confused as well, i guess its windows-related :shrug:

@charliermarsh @zanieb 
Thanks for all your hard work by the way, `uv` is **awesome**!

---

_Comment by @bluss on 2024-10-04 15:42_

Thanks @linde12, you explained some of the problems I've seen in windows CI. I'll test the link mode as a solution to them.. (edit: yes :slightly_smiling_face: )

---

_Comment by @zanieb on 2024-10-04 16:21_

It sort of feels like we should just force copy of these files :/ I don't love it though

---

_Comment by @kahojyun on 2024-10-04 16:21_

Unable to reproduce on my computer. I can delete the whole uv cache folder successfully with matplotlib window opened, and the `File Locksmith` utility from powertoys shows that lots of pyd files under matplotlib and numpy are in use.
This might be related to other factors like Windows version or Windows defender scanning the newly created venv.

---

_Comment by @linde12 on 2024-10-04 16:38_

@kahojyun mind running the example just to make sure? I was able to reproduce it on the two different windows 10 (i don't run windows myself and can't test on win11) machines, both when running mingw bash and regular powershell. One with python 3.8 and the other python 3.10 (just to make sure it wasn't python acting up).

I used `strace` (mingw) and could see that the `.pyd` was being loaded (regardless if i imported `markupsafe` or not) and as soon as that happened i could delete the entire `.venv` except the `.pyd` file.

Maybe also, to be extra sure, set the `UV_LINK_MODE` to `hardlink` explicitly? E.g. `UV_LINK_MODE=hardlink strace python main.py`

---

_Comment by @kahojyun on 2024-10-04 16:59_

Tried with powershell 7 on win11:

```
~\uvtest
❯ rm env:UV_CACHE_DIR

~\uvtest
❯ $env:UV_LINK_MODE="hardlink"

~\uvtest
❯ $env:UV_PYTHON="3.8"

~\uvtest
❯ uv venv .venv
Using CPython 3.8.20
Creating virtual environment at: .venv
Activate with: .venv\Scripts\activate

~\uvtest
❯ uv pip install markupsafe
Resolved 1 package in 1.16s
Prepared 1 package in 90ms
Installed 1 package in 10ms
 + markupsafe==2.1.5

~\uvtest
❯ nvim main.py

~\uvtest via 🐍 v3.12.6 took 9s
❯ uv run main.py
Using CPython 3.8.20
Creating virtual environment at: .venv
Activate with: .venv\Scripts\activate
Audited 1 package in 1ms

~\uvtest via 🐍 v3.12.6
❯ .\.venv\Scripts\python.exe .\main.py
Using CPython 3.8.20
Creating virtual environment at: .venv
Activate with: .venv\Scripts\activate
Resolved 1 package in 7ms
Installed 1 package in 10ms
 + markupsafe==2.1.5
```

I don't have mingw and bash to try the exact command.

---

_Comment by @linde12 on 2024-10-04 17:02_

Interesting, maybe the .pyd in question isnt being loaded

---

_Comment by @linde12 on 2024-10-04 17:17_

Here is a repo with two actions. One running win-11 and one win-10. It uses the example, but now i also import and use `markupsafe` (because it worked fine when i did not import it apparently, not sure why i got different behaviors)

https://github.com/linde12/uv-py-hardlink-windows

![image](https://github.com/user-attachments/assets/905df2d2-b7ca-4cf3-bcbb-5b454e2ca0b4)

![image](https://github.com/user-attachments/assets/28642dc8-4ab2-4245-b126-c02f1503f0fb)

---

_Comment by @kahojyun on 2024-10-04 19:41_

> Here is a repo with two actions. One running win-11 and one win-10. It uses the example, but now i also import and use `markupsafe` (because it worked fine when i did not import it apparently, not sure why i got different behaviors)
> 
> https://github.com/linde12/uv-py-hardlink-windows
> 
I tried tweaking these workflows and found that [running with pwsh instead of bash exited normally](https://github.com/kahojyun/uv-py-hardlink-windows/actions/runs/11185535371). Weird

---

_Comment by @linde12 on 2024-10-04 21:11_

Weird yeah... https://github.com/linde12/uv-py-hardlink-windows/actions/runs/11187011758/job/31103231396

Without strace it works? I'm too tired for this... I'm almost sure i observed the same issue in pwsh too, but i might be wrong

---

_Comment by @chrisrodrigue on 2024-10-05 01:05_

> It sort of feels like we should just force copy of these files :/ I don't love it though

Even though copying seems suboptimal I think it’s actually a good working solution. 

(Along with the omission of Bash, Microsoft’s decision to restrict symlink creation to elevated users in Windows seems like it was deliberately intended to break OS interoperability.)

---

_Comment by @chrisrodrigue on 2024-10-05 01:11_

Symlinks made in WSL also don’t work… could have been a cool workaround :/ 

https://blog.trailofbits.com/2024/02/12/why-windows-cant-follow-wsl-symlinks/

Although… 
> One last thing to point out is that when Bill Demirkapi tested this behavior, he noticed that Windows could follow WSL’s symlinks when they were created with a relative path but not with an absolute path. On all systems I tested, Windows couldn’t follow any symlinks created by WSL. So there is still some mystery left here to investigate.

---

_Comment by @linde12 on 2024-10-06 15:30_

I modified my script slightly to compare to my real usecase.

I create a `noxfile.py` which installs markupsafe and runs some script that uses it, then i run `nox -f noxfile.py` and try to delete the `testdir` after this. This fails, even without `strace` and using pwsh: https://github.com/linde12/uv-py-hardlink-windows/actions/runs/11203062664/job/31139882483

---

_Comment by @kahojyun on 2024-10-06 17:24_

It seems that [if any program opens the file without `FILE_SHARE_DELETE` flag, all of the hardlink-peers can't be deleted](https://superuser.com/questions/678357/how-to-delete-windows-ntfs-hard-link-mklink-h-while-original-is-in-use). If we want to avoid this kind of error we have to use symlink or copy.

(I'm okay with copy by default as long as [Microsoft brings automatic CoW to Dev Drive](https://devblogs.microsoft.com/engineering-at-microsoft/copy-on-write-in-win32-api-early-access/).) 

---

_Comment by @linde12 on 2024-10-06 17:55_

> It seems that [if any program opens the file without `FILE_SHARE_DELETE` flag, all of the hardlink-peers can't be deleted](https://superuser.com/questions/678357/how-to-delete-windows-ntfs-hard-link-mklink-h-while-original-is-in-use). If we want to avoid this kind of error we have to use symlink or copy.
> 
> (I'm okay with copy by default as long as [Microsoft brings automatic CoW to Dev Drive](https://devblogs.microsoft.com/engineering-at-microsoft/copy-on-write-in-win32-api-early-access/).)

I was actually reading the same post the other day, but was confused as to why we were seeing different behaviors. Especially when i was using `strace` :eyes: Copying the `.pyd` might be the least bad fix IMO

---

_Referenced in [astral-sh/uv#7990](../../astral-sh/uv/issues/7990.md) on 2024-10-08 13:15_

---

_Comment by @parched on 2025-12-03 00:56_

We're hitting this too, we have a windows machine where end-to-end testing regularly creates then deletes a lot of venvs but we also have a python process permanently running that uses all the same packages. The venv tear down is failing leaving all these half empty venvs full of `pyd`s

What's needed to get this issue fixed? As far as I understand, based on the links above, uv shouldn't expect hardlinking to work on Windows. Should I submit a PR changing the default `link-mode` to `copy` on Windows?

---
