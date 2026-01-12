```yaml
number: 10288
title: Windows AV software causes 0.5 second delay for unsigned binaries
type: issue
state: open
author: paugier
labels:
  - windows
assignees: []
created_at: 2025-01-03T14:30:37Z
updated_at: 2025-12-15T17:05:26Z
url: https://github.com/astral-sh/uv/issues/10288
synced_at: 2026-01-12T16:00:10Z
```

# Windows AV software causes 0.5 second delay for unsigned binaries

---

_@paugier_

I observe a very slow Python startup in venv created by UV.

```sh
uv venv my-uv-venv
.\my-uv-venv\Scripts\activate
Measure-Command { python --version }
# slow (~0.5 second)
Measure-Command { .\venv-uv\Scripts\python.exe --version }
# slow (~0.5 second)
Measure-Command { .\AppData\Roaming\uv\python\cpython-3.13.1-windows-x86_64-none\python.exe --version }
# quite fast (~0.06 second)
```

The same kind of startup delays are observed for Python applications installed with `uv tools`.

I start to understand after some experiments and discussions (https://discuss.python.org/t/about-exe-wrappers-created-by-frontends-when-installing-wheels-on-windows/75942/8) that it is related to a virus scan triggered each time the .exe files are launched.

This delay is related to this particular computer with a antivirus software doing some checks when modified .exe are launched. I guess it is a quite common situation for UV users on Windows. It would be nice to find a solution. Possibly UV could do something to produce files that are not checked at startup by antivirus software. Alternatively, it would be nice to document how to avoid these startup delays.


---

_Label `windows` added by @charliermarsh on 2025-01-03 15:27_

---

_Comment by @charliermarsh on 2025-01-03 15:28_

I'm not sure that there's much we can do here. Did you learn anything that we can apply concretely from the discussion.python.org thread?

---

_Comment by @samypr100 on 2025-01-03 23:32_

Wow! I wonder what those slow AV's are searching for / doing. These are the results on my Windows system.

```
(uv) ➜ uv git:(main) Measure-Command { python --version }

Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 0
Milliseconds      : 47
Ticks             : 473388
TotalDays         : 5.47902777777778E-07
TotalHours        : 1.31496666666667E-05
TotalMinutes      : 0.00078898
TotalSeconds      : 0.0473388
TotalMilliseconds : 47.3388

(uv) ➜ uv git:(main) Measure-Command { %USERPROFILE%\AppData\Local\Programs\Python\Python312\python.exe --version }

Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 0
Milliseconds      : 44
Ticks             : 440542
TotalDays         : 5.09886574074074E-07
TotalHours        : 1.22372777777778E-05
TotalMinutes      : 0.000734236666666667
TotalSeconds      : 0.0440542
TotalMilliseconds : 44.0542
```

---

_Comment by @notatallshaw on 2025-01-03 23:47_

I once worked in a large Enterprise that was testing a security tool on Windows that implemented an allow list, however the allow list wasn't stored on the computer, a hash of the executable was made and sent over the network on each process open.

Further, when starting multiple processes simultaneously the security tool’s response time scaled superlinearly, I had examples, like opening PyCharm, where it took over 30 seconds and started to time out (I, and several other developers, successfully made the case that this meant it was unworkable on our machines). It's a wild world of latency inducing security tools out there.

---

_Comment by @samypr100 on 2025-01-04 01:32_

>  I had examples, like opening PyCharm, where it took over 30 seconds and started to time out 

Indeed, I've also been there a couple of times 😭 

---

_Comment by @paugier on 2025-01-04 06:17_

I don't come from Windows, so I don't understand everything but it seems (https://discuss.python.org/t/slow-startup-on-windows-for-virtual-environments/75758/27) that it is important to

- sign the binaries
- copy python.exe without modification for venvs

Indeed virtual envs from Python installed from the official installers do not suffer from startup delays. `python.exe` in virtual envs is just a copy and just check a text file `pyvenv.cfg` next to it.

For `uv tools`, find a great solution not involving modifying .exe files 🙂

I don't yet now how to do this but it seems that for some antivirus programs, one can add a kind of white list of programs or directories (see https://discuss.python.org/t/slow-startup-on-windows-for-virtual-environments/75758/29). If it is indeed possible for popular security software, it might be nice to add a note on UV tools documentation.

---

_Comment by @samypr100 on 2025-01-04 23:04_

> sign the binaries

Fair, python build standalone binaries would benefit from signing

> For uv tools, find a great solution not involving modifying .exe files 🙂

Which .exe files? Executable scripts launchers are generated on the fly, they're not pre-existing.


---

_Comment by @paugier on 2025-01-07 15:53_

Note that it also apply to Rust applications like ruff. Ruff installed with UV is very slow on this computer.

```
Measure-Command { .\AppData\Roaming\uv\tools\ruff\Scripts\ruff.exe --version }
TotalMilliseconds : 782,1066
```

Moreover, I confirm that it is due to an antivirus software (see https://discuss.python.org/t/slow-startup-on-windows-for-virtual-environments/75758/34) and that the solutions are to sign binaries (python.exe in the venvs, launchers for python entrypoints, Rust applications like ruff.exe, etc.) and not modify binaries with shebang (what is done for launchers for python entrypoints). Instead of modifying binaries with shebang, one could use 2 files (black.exe and black-script.json for example).

---

_Renamed from "Windows: 0.5 second delay before startup for venv and tools" to "Windows AV software causes 0.5 second delay for unsigned binaries" by @konstin on 2025-12-15 17:05_

---
