---
number: 2074
title: Editable package install fails after a previous uninstall
type: issue
state: closed
author: amirhosseindavoody
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2024-02-29T07:35:43Z
updated_at: 2024-03-07T04:35:23Z
url: https://github.com/astral-sh/uv/issues/2074
synced_at: 2026-01-07T13:12:17-06:00
---

# Editable package install fails after a previous uninstall

---

_Issue opened by @amirhosseindavoody on 2024-02-29 07:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Steps to reproduce
* Activate a conda environment.
* Install a local package that I have using `uv pip install -e .`
* Uninstall the local package using `uv pip uninstall -e .`
* Install the local package again using `uv pip install -e .`

I get the following error:
```
error: Cannot uninstall package; RECORD file not found at: path/to/site-packages/mylib-0.1.dist-info/RECORD
```

If I manually remove `path/to/site-packages/mylib-0.1.dist-info` directory, then I am able to install my local package again.

---

_Label `bug` added by @charliermarsh on 2024-02-29 14:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-29 14:39_

---

_Comment by @charliermarsh on 2024-02-29 14:39_

Thanks! Probably my bug from the most recent release.

---

_Comment by @charliermarsh on 2024-02-29 15:19_

Do you get this consistently? I wasn't able to reproduce.

---

_Label `needs-mre` added by @charliermarsh on 2024-02-29 15:19_

---

_Comment by @amirhosseindavoody on 2024-02-29 17:56_

I tried this with a fresh environment and the same error happened.

Some more details about my environment setup
* I am using `pixi` as the package manager.
* Then I install `uv` for my specific environment using `pixi add uv`
* Then I use `pixi shell` to activate the environment.
* Now I run the `uv pip install -e \path\to\my\local\package`
* The local package is using pyproject.toml for setup. Something similar to [this]( https://github.com/nedbat/pkgsample/blob/main/pyproject.toml)

---

_Comment by @charliermarsh on 2024-02-29 18:01_

Thanks, will try again.

---

_Comment by @charliermarsh on 2024-02-29 18:12_

This still worked for me even when running through pixi. Is there anything else you can share about your OS, etc.?

---

_Comment by @amirhosseindavoody on 2024-02-29 19:01_

My OS is Suse 12.

I don't know what else might help.

If this is not reproducible we can close this and others will open something if it happens again.

---

_Comment by @charliermarsh on 2024-02-29 20:50_

I believe it's a real problem if you're seeing it consistently. I just need to find a way to repro.

---

_Comment by @iwane-pl on 2024-03-01 07:22_

We've run on the same error when installing `lxml`, for example. The procedure was
```
PS C:\Users\me> C:\Python3.12\python.exe -m venv venv  
PS C:\Users\me> .\venv\Scripts\activate.ps1                                                                                    
(venv) PS C:\Users\me> uv pip install lxml==5.1.0                                                                              
Resolved 1 package in 4ms                                                                                                       
Installed 1 package in 50ms                                                                                                     
 + lxml==5.1.0                                                                                                                  
(venv) PS C:\Users\me> uv pip install lxml==4.9.3                                                                              
Resolved 1 package in 4ms                                                                                                       
Installed 1 package in 51ms                                                                                                     
 - lxml==5.1.0                                                                                                                  
 + lxml==4.9.3                                                                                                                  
(venv) PS C:\Users\me> uv pip install lxml==5.1.0                                                                              
Resolved 1 package in 4ms                                                                                                       
error: Cannot uninstall package; RECORD file not found at: C:\Users\me\venv\Lib\site-packages\lxml-5.1.0.dist-info\RECORD 
```
OS info:
```
# Windows Server 2016
> (Get-CimInstance Win32_OperatingSystem).version
10.0.14393    
> python --version                                                                                        
Python 3.12.2
> uv --version                                                                                            
uv 0.1.12 (f68b2d1d5 2024-02-28)
```

Don't ask why we install it that way :) (hint: multiple requirement files in sequence)
Didn't happen on Linux, though.

EDIT: Still present on 0.1.13, despite #2091 

---

_Comment by @konstin on 2024-03-01 10:40_

I tried on my windows desktop but i can't reproduce it there

```
PS C:\Users\Konsti\projects\uv> python.exe -m venv venv 
PS C:\Users\Konsti\projects\uv> .\venv\Scripts\activate.ps1            
(venv) PS C:\Users\Konsti\projects\uv> powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
Downloading uv 0.1.13 (x86_64-pc-windows-msvc)
Installing to C:\Users\Konsti\.cargo\bin
  uv.exe
Everything's installed!
(venv) PS C:\Users\Konsti\projects\uv> uv --version
uv 0.1.13 (9ce5170e6 2024-02-29)
(venv) PS C:\Users\Konsti\projects\uv>  uv pip install lxml==5.1.0                                 

Resolved 1 package in 228ms
Downloaded 1 package in 366ms
Installed 1 package in 65ms
 + lxml==5.1.0
(venv) PS C:\Users\Konsti\projects\uv> uv pip install lxml==4.9.3                         
Resolved 1 package in 170ms
Downloaded 1 package in 473ms
Installed 1 package in 59ms
 - lxml==5.1.0
 + lxml==4.9.3
(venv) PS C:\Users\Konsti\projects\uv> uv pip install lxml==5.1.0                 
Resolved 1 package in 2ms
Installed 1 package in 68ms
 - lxml==4.9.3
 + lxml==5.1.0
```

---

_Comment by @iwane-pl on 2024-03-01 12:02_

:thinking: 
I tried on other Windows versions too. Same `uv` (0.1.13) and Python (3.12.2).

```
# Win10

PS C:\Users\me> (Get-CimInstance Win32_OperatingSystem).version
10.0.19045
PS C:\Users\me> C:\Python3.12\python.exe -m venv venv
PS C:\Users\me> .\venv\Scripts\activate.ps1
(venv) PS C:\Users\me> uv pip install lxml==5.1.0
Resolved 1 package in 84ms
Downloaded 1 package in 150ms
Installed 1 package in 82ms
 + lxml==5.1.0
(venv) PS C:\Users\me> uv pip install lxml==4.9.3
Resolved 1 package in 47ms
Downloaded 1 package in 156ms
Installed 1 package in 107ms
 - lxml==5.1.0
 + lxml==4.9.3
(venv) PS C:\Users\me> uv pip install lxml==5.1.0 
Resolved 1 package in 6ms
Installed 1 package in 75ms
 - lxml==4.9.3
 + lxml==5.1.0


# WinServer2019

PS C:\Users\me> (Get-CimInstance Win32_OperatingSystem).version
10.0.17763
PS C:\Users\me> C:\Python3.12\python.exe -m venv venv
PS C:\Users\me> .\venv\Scripts\activate.ps1
(venv) PS C:\Users\me> uv pip install lxml==5.1.0
Resolved 1 package in 84ms
Downloaded 1 package in 199ms
Installed 1 package in 66ms
 + lxml==5.1.0
(venv) PS C:\Users\me> uv pip install lxml==4.9.3
Resolved 1 package in 33ms
Downloaded 1 package in 224ms
Installed 1 package in 68ms
 - lxml==5.1.0
 + lxml==4.9.3
(venv) PS C:\Users\me> uv pip install lxml==5.1.0
Resolved 1 package in 7ms
error: Cannot uninstall package; RECORD file not found at: C:\Users\me\venv\Lib\site-packages\lxml-5.1.0.dist-info\RECORD 

# WinServer2022

PS C:\Users\me> (Get-CimInstance Win32_OperatingSystem).version
10.0.20348
PS C:\Users\me> C:\Python3.12\python.exe -m venv venv  
PS C:\Users\me> .\venv\Scripts\activate.ps1
(venv) PS C:\Users\me> uv pip install lxml==5.1.0
Resolved 1 package in 187ms
Downloaded 1 package in 158ms
Installed 1 package in 88ms
 + lxml==5.1.0
(venv) PS C:\Users\me> uv pip install lxml==4.9.3
Resolved 1 package in 35ms
Downloaded 1 package in 142ms
Installed 1 package in 87ms
 - lxml==5.1.0
 + lxml==4.9.3
(venv) PS C:\Users\me> uv pip install lxml==5.1.0
Resolved 1 package in 21ms
Installed 1 package in 110ms
 - lxml==4.9.3
 + lxml==5.1.0

```

It seems like it reproduces on older Windows versions. Updated Win10 and WinServer2022 seem unaffected, while WinServer2016 and 2019 fail.

---

_Comment by @gozdal on 2024-03-06 18:00_

As @iwane-pl noticed, this happens on Windows 2016 and 2019 but not on Windows 10 and 2022.
The difference is that after an uninstall an empty directory is left befind. E.g. if you have `lxml==5.1.0` installed and you downgrade to `lxml==4.9.3` then `site-packages` will have
```
venv\Lib\site-packages\lxml-4.9.3.dist-info
venv\Lib\site-packages\lxml-5.1.0.dist-info
```

Where 5.1.0 dir will be empty and hence the error
```
error: Cannot uninstall package; RECORD file not found at: venv\Lib\site-packages\lxml-5.1.0.dist-info\RECORD
```

Curiously enough it happens only on selected Windows versions.

---

_Comment by @charliermarsh on 2024-03-06 18:02_

My guess is that this is due to delayed file deletion...? E.g., we delete a file, then test if a directory is empty, and delete the directory if so; but I believe there can be temporary inconsistencies, such that when you delete a file, it may still "exist" for some period of time, or something like that.

---

_Comment by @gozdal on 2024-03-06 18:14_

It's 100% reproducible, so I'd say it's not a race. Could try Process Monitor tomorrow to see what's happening.

---

_Comment by @charliermarsh on 2024-03-06 18:23_

Yeah what I'm describing could still be consistent with it being 100% reproducible.

---

_Comment by @charliermarsh on 2024-03-06 18:24_

I don't have a system to test this on, so the best I can do is try a refactor and ask someone here to test it.

---

_Comment by @charliermarsh on 2024-03-07 03:22_

Good news, I'm able to reproduce this on Windows 2019 in GitHub Actions! https://github.com/astral-sh/uv/pull/2259

---

_Referenced in [astral-sh/uv#2259](../../astral-sh/uv/pulls/2259.md) on 2024-03-07 04:30_

---

_Comment by @charliermarsh on 2024-03-07 04:35_

Ok, I believe this is fixed by https://github.com/astral-sh/uv/pull/2259. If you're able to reproduce after the next release, please let me know.

---

_Closed by @charliermarsh on 2024-03-07 04:35_

---
