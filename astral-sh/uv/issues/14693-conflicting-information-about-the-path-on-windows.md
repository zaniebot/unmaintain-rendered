---
number: 14693
title: Conflicting information about the PATH on Windows
type: issue
state: open
author: konstin
labels:
  - bug
  - windows
assignees: []
created_at: 2025-07-17T20:50:00Z
updated_at: 2025-07-30T15:04:29Z
url: https://github.com/astral-sh/uv/issues/14693
synced_at: 2026-01-10T01:25:47Z
---

# Conflicting information about the PATH on Windows

---

_Issue opened by @konstin on 2025-07-17 20:50_

In my Windows PowerShell, uv 0.8.0 (branch) can't decide whether the bin directory is already on path or not:

```
PS C:\Users\Konsti\projects-ext\uv> target\debug\uv python install --reinstall 3.13
Installed Python 3.13.5 in 12.84s
 + cpython-3.13.5-windows-x86_64-none (python3.13.exe)
warning: `C:\Users\Konsti\.local\bin` is not on your PATH. To use installed Python executables, run `$env:PATH = "C:`\Users`\Konsti`\.local`\bin;$env:PATH"` or `uv python update-shell`.
PS C:\Users\Konsti\projects-ext\uv> target\debug\uv tool update-shell              
Executable directory C:\Users\Konsti\.local\bin is already in PATH
```

According to the PATH and the GUI that shows the same information, the bin path is not on PATH:

```
PS $Env:PATH
C:\Users\Konsti\.cargo\bin;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\W
indowsPowerShell\v1.0\;C:\WINDOWS\System32\OpenSSH\;C:\Program Files\nodejs\;C:\ProgramData\chocolatey\b
in;C:\Program Files\Git\cmd;C:\Program Files\Docker\Docker\resources\bin;C:\Program Files\PowerShell\7\;
C:\Users\Konsti\AppData\Local\Programs\Python\Launcher\;C:\Users\Konsti\.pyenv\pyenv-win\bin;C:\Users\Ko
nsti\.pyenv\pyenv-win\shims;C:\Users\Konsti\AppData\Local\Programs\Python\Python38\Scripts\;C:\Users\Kon
sti\AppData\Local\Programs\Python\Python38\;C:\Users\Konsti\AppData\Local\Programs\Python\Python312\Scri
pts\;C:\Users\Konsti\AppData\Local\Programs\Python\Python312\;C:\Users\Konsti\.cargo\bin;C:\Users\Konsti
\AppData\Local\Microsoft\WindowsApps;C:\Users\Konsti\AppData\Local\JetBrains\Toolbox\scripts;C:\Users\Ko
nsti\AppData\Roaming\npm;C:\Users\Konsti\AppData\Local\Programs\Microsoft VS Code\bin;C:\Users\Konsti\AppData\Local\Microsoft\WindowsApps;
```

---

_Comment by @zanieb on 2025-07-17 20:54_

That's.. interesting. I presume that's a longstanding bug.

---

_Label `bug` added by @zanieb on 2025-07-17 20:54_

---

_Label `windows` added by @konstin on 2025-07-17 20:57_

---

_Comment by @konstin on 2025-07-17 20:59_

It already fails with 0.7.0 (with `uv tool update-shell`), but it surfaced with the testing now.

---

_Comment by @karnawhat on 2025-07-25 15:51_

+1 I also faced this issue with `uv==0.7.14`. Perhaps the check needs to be done against the specific shell currently running?

I was trying to update the `PATH` for the `C:\windows\system32\cmd.exe` "shell" using `uv tool update-shell`, and ran into the similar issue.
```sh
$ echo %COMSPEC%
C:\windows\system32\cmd.exe

$ uv tool update-shell
Executable directory C:\Users\N764265\.local\bin is already in PATH

$ echo %PATH%
C:\Program Files (x86)\Micro Focus\Reflection\;C:\Program Files\Micro Focus\Reflection\;C:\Program Files (x86)\Common Files\Oracle\Java\javapath;C:\windows\system32;C:\windows;C:\windows\System32\Wbem;C:\windows\System32\WindowsPowerShell\v1.0\;C:\Program Files\aim\aim-install\bin\;C:\Program Files\dotnet\;C:\Program Files\PKWARE\SCCLI;C:\Program Files\Citrix\System32\;C:\Program Files\Citrix\ICAService\;C:\Users\N764265\AppData\Local\Microsoft\WindowsApps;
```

but checking the PATH for the `cmd.exe` shell, we see that the bin directory is not included in that PATH and the message that the bin directory is already in the path is a bit misleading. However, it seems like it updates the environment variables for the Windows PowerShell, as that bin directory is available in its PATH?

```sh
PS H:\> $ENV:PATH
C:\Program Files (x86)\Micro Focus\Reflection\;C:\Program Files\Micro Focus\Reflection\;C:\Program Files (x86)\Common Files\Oracle\Java\javapath;C:\windows\system32;C:\windows;C:\windows\System32\Wbem;C:\windows\System32\WindowsPowershell\v1.0\;C:\Program Files\aim\aim-install\bin\;C:\Program Files\dotnet\;C:\Program Files\PKWARE\SCCLI;C:\Program Files\Citrix\System32\;C:\ProgramFiles\Citrix\ICAService\;C:\Users\N764265\.local\bin;C:\Users\N764265\AppData\Local\Microsoft\WindowsApps
```

If possible, can the message be improved to say which shell the PATH was updated for?


---

_Comment by @karnawhat on 2025-07-28 15:34_


**EDIT:** It turns out the `%PATH%` variable was updated globally, but was not reflected in the windows command prompt `cmd.exe` even after restarting the shell as recommended by `uv tool update-shell`. The reason, it turns out, is due to VSCode not updating the environment variables unless you **restart the whole workspace**, despite sourcing and/or opening a new shell (https://github.com/microsoft/vscode/issues/47816).

When re-opened, the terminal in VS Code now has the updated PATH, e.g.
```sh
$ echo %PATH%
C:\Program Files (x86)\Micro Focus\Reflection\;C:\Program Files\Micro Focus\Reflection\;C:\Program Files (x86)\Common Files\Oracle\Java\javapath;C:\windows\system32;C:\windows;C:\windows\System32\Wbem;C:\windows\System32\WindowsPowershell\v1.0\;C:\Program Files\aim\aim-install\bin\;C:\Program Files\dotnet\;C:\Program Files\PKWARE\SCCLI;C:\Program Files\Citrix\System32\;C:\ProgramFiles\Citrix\ICAService\;C:\Users\N764265\.local\bin;C:\Users\N764265\AppData\Local\Microsoft\WindowsApps
```

cc @konstin @zanieb 

---

_Comment by @zanieb on 2025-07-28 16:04_

Can we detect that we are in a VSCode terminal?

---

_Comment by @konstin on 2025-07-28 16:39_

Maybe we can check `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment` and `HKEY_CURRENT_USER\Environment`? If the registry and the current session disagree over whether the uv python directory is in PATH, we can recommend to restart the application.

---

_Comment by @karnawhat on 2025-07-30 14:59_

Thanks @konstin and @zanieb for the quick reply! Please let me know if you would want me to attempt a fix. Happy to contribute back! I'm guessing [this](https://github.com/astral-sh/uv/blob/17f0c91896d43b66851229d26385a6989d9e71b2/crates/uv/src/commands/tool/update_shell.rs#L18) script would be the script to modify?

---

_Comment by @zanieb on 2025-07-30 15:04_

Happy to review a fix!

I think that makes sense. We might want to make a change in `Shell::contains_path` too? I'm not sure though. It could be a separate goal to improve the `warn_if_not_on_path` messaging.

---
