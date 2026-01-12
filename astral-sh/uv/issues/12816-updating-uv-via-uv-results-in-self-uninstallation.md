```yaml
number: 12816
title: Updating uv via uv results in self-uninstallation and broken reinstall behavior
type: issue
state: closed
author: CoderJackZhu
labels:
  - bug
  - windows
assignees: []
created_at: 2025-04-10T17:35:55Z
updated_at: 2025-06-27T16:17:19Z
url: https://github.com/astral-sh/uv/issues/12816
synced_at: 2026-01-12T16:01:13Z
```

# Updating uv via uv results in self-uninstallation and broken reinstall behavior

---

_@CoderJackZhu_

### Summary

When attempting to update uv using itself (i.e., by running a command like  uv self update), the update process causes uv to uninstall itself. After this, attempting to run uv results in a "command not found" error.

Further attempts to reinstall uv using a method like  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex" will only download the package but will not actually install it, leading to a broken state that requires manual intervention to fix.

![Image](https://github.com/user-attachments/assets/e8adca7c-59e4-471e-a291-3972ee356668)

### Platform

Win 11 x86_64

### Version

uv 0.6.5

### Python version

Python 3.12

---

_Label `bug` added by @CoderJackZhu on 2025-04-10 17:35_

---

_Label `windows` added by @zanieb on 2025-04-10 17:52_

---

_Comment by @zanieb on 2025-04-10 17:52_

cc @Gankra 

---

_Comment by @Gankra on 2025-04-10 18:56_

Yikes! 

* How did you originally install the package? 
* Did you use any special environment variables or manually relocate the binary?
* If you download the .ps1 script and run it with `-Verbose` does it display any additional information?

---

_Assigned to @Gankra by @Gankra on 2025-04-10 18:57_

---

_Comment by @CoderJackZhu on 2025-04-11 02:58_

Initially, I installed it directly via PowerShell with:
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
Later, I found that uv self update failed. Then I installed it via pip:
pip install uv
This worked, but the binaries uv.exe and uvx.exe were not installed under C:\Users\tremble\.local\bin. The earlier version was placed there, but after using pip install uv, the binaries ended up in C:\Users\tremble\miniconda3\Scripts or C:\Users\tremble\miniconda3\Lib\site-packages. I found this odd, so I manually moved uv.exe and uvx.exe to C:\Users\tremble\.local\bin, and they worked normally. However, uv self update still fails—executing it causes the binaries to be deleted.

There was no additional manual path configuration. I just checked my environment variables, and there are no extra settings.

Now, when installing with -Verbose, I cannot find the detailed log files in the specified path.

![Image](https://github.com/user-attachments/assets/c74f53e7-12be-429f-ab05-0dc795331bef)

![Image](https://github.com/user-attachments/assets/302a4660-d79a-43e9-886d-f1f0f83f2684)

---

_Comment by @CoderJackZhu on 2025-05-17 06:41_

This problem has now been resolved

---

_Closed by @konstin on 2025-05-19 08:01_

---

_Reopened by @konstin on 2025-05-19 08:01_

---

_Closed by @Gankra on 2025-06-27 16:17_

---
