```yaml
number: 10030
title: "Getting \"access is denied\" in user folder when running \"uv tool install\" on Windows 10"
type: issue
state: closed
author: driscollis
labels: []
assignees: []
created_at: 2024-12-19T14:32:54Z
updated_at: 2025-09-10T17:23:17Z
url: https://github.com/astral-sh/uv/issues/10030
synced_at: 2026-01-12T16:00:05Z
```

# Getting "access is denied" in user folder when running "uv tool install" on Windows 10

---

_@driscollis_

I tried the [posting](https://github.com/darrenburns/posting) app with uv on my Windows 10 machine. The author of `posting` recommends running the following command:

`uv tool install --python 3.12 posting`

When I run that in a regular Windows Terminal or a Windows Terminal with admin privileges, I get the following error:

```
C:\workspace\> uv tool install --python 3.12 posting
Resolved 35 packages in 5.75s
   Built pyperclip==1.9.0
Prepared 34 packages in 4.25s
error: Failed to install: httpx-0.28.1-py3-none-any.whl (httpx==0.28.1)
  Caused by: Failed to persist temporary file to C:\Users\Mike\AppData\Roaming\uv\tools\posting\Lib\site-packages\..\..\Scripts\httpx.exe: Access is denied. (os error 5)
```

I tried this on two different Windows 10 machines with the same result. I tried running `uv cache clean` and re-ran the above command with the same result.

I am using uv 0.5.10 (37b11ddb2 2024-12-17)

---

_Comment by @FishAlchemist on 2024-12-19 15:28_

I seem to have seen this issue before. Can you tell me the output of running the following command on powershell?
```powershell
Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem' -Name 'LongPathsEnabled'
```
It's just a command to read the registry, so it doesn't require administrator privileges.

---

_Comment by @driscollis on 2024-12-19 15:42_

Here's what I get:

LongPathsEnabled : 0
PSPath           : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem
PSParentPath     : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control
PSChildName      : FileSystem
PSDrive          : HKLM
PSProvider       : Microsoft.PowerShell.Core\Registry

---

_Comment by @FishAlchemist on 2024-12-19 16:13_

I'm not sure if this issue is triggered by an overly long path. Perhaps you could try enabling long paths.
https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=powershell#registry-setting-to-enable-long-paths
But if this is truly the issue, I'm not sure if UV has a way to detect it.

Edit:
Even after turning off long paths in my Windows, it still seems to work, so it's probably not the issue.
Perhaps you could also check if the denied-access folder can be accessed normally without granting additional permissions.


---

_Comment by @driscollis on 2024-12-19 16:27_

I tried enabling long paths, but I still get the same error.

There are no subfolders in `C:\Users\Mike\AppData\Roaming\uv\tools` so I am guessing uv is cleaning up when it hits that error. That is unfortunate since I can't inspect that folder to see what the permissions are or if I can access it

---

_Comment by @samypr100 on 2024-12-21 01:29_

~~Hmmm, I actually would expect uv to install it in `C:\Users\Mike\.local\bin\posting.exe` which is where it does on my Windows system based on my home path. Was this working for you previously?~~ Edit: I think in your case it even fails to get there.

Could you capture some trace logs?

```powershell
$Env:RUST_LOG = 'uv=trace'
uv tool uninstall --python 3.12 posting
```

---

_Label `needs-mre` added by @samypr100 on 2024-12-21 01:29_

---

_Comment by @Cogito on 2025-04-16 01:44_

Check if windows defender is blocking recently installed executables. Apparently it blocks some 'risky behaviour' like a fresh executable, and may stop blocking it 24 hours later. Adding the relevant folders to an exclusion list should help if this is the case.

---

_Comment by @driscollis on 2025-07-02 14:25_

It looks like this got resolved somehow. I was trying to reproduce it again and wasn't able to with that version of uv. I also tried the latest version of uv (0.7.18) and it seems to work on my Windows 10 box now. 

I am still suspicious that this is a Windows Defender issue of some sort, but  that is difficult to prove right now

---

_Closed by @konstin on 2025-08-21 11:58_

---

_Label `needs-mre` removed by @konstin on 2025-08-21 11:58_

---

_Comment by @cleb98 on 2025-09-10 17:23_

i get the same error on window 10, i can create venv, with numpy for instance, but if i try pyperclip i fall into this error. 
The strange things is that in just one venv i can install pyperclip, in all the other i got the same beaviour. 
# test
## i already have one venv with pyperclip installed

Cristian.BELLUCCI ~/python_exp/venv-test/venv2
$ uv pip install pyperclip
Resolved 1 package in 49ms
  x Failed to download and build `pyperclip==1.9.0`
  |-> Failed to read from the distribution cache
  `-> failed to open file
      `C:\Users\Cristian.BELLUCCI\AppData\Local\uv\cache\builds-v0\.tmpYgNgHP\py
perclip-1.9.0-py3-none-any.whl`:
      Access is denied. (os error 5)

this is what i get if i try to install pandas in the same way:

Cristian.BELLUCCI ~/python_exp/venv-test/venv2
$ uv pip install pandas
Resolved 6 packages in 1.22s
Prepared 5 packages in 4.28s
Installed 5 packages in 1.84s
 + pandas==2.3.2
 + python-dateutil==2.9.0.post0
 + pytz==2025.2
 + six==1.17.0
 + tzdata==2025.2






---
