---
number: 5066
title: uv has problems with pyspark depennding on whether it comes via a private feed or not
type: issue
state: closed
author: fralik
labels: []
assignees: []
created_at: 2024-07-15T08:27:01Z
updated_at: 2024-07-16T15:03:20Z
url: https://github.com/astral-sh/uv/issues/5066
synced_at: 2026-01-07T13:12:17-06:00
---

# uv has problems with pyspark depennding on whether it comes via a private feed or not

---

_Issue opened by @fralik on 2024-07-15 08:27_

Hi!
I observed an interesting bevavior that uv fails to install pyspark depending on whether distribution comes directly from pip or from our private feed. Both downloaded packages are identical (I checked by manually downloading them from both indices and comparing).
Perhaps the problem is in pyspark itself as it is distributed in sources rather than a wheel.

Attached are two log files obtained like this:
```
C:\Users\fralik\AppData\Local\miniconda3\envs\myvenv-39-uv\python.exe -m uv pip install "pyspark==3.5.1" --verbose --no-cache > log_succcess.txt 2>&1
```

The failed command is similar, but is executed from a folder containing `uv.toml`:
```
[pip]
keyring-provider = "subprocess"
index-url = "https://VssSessionToken@myorg.pkgs.visualstudio.com/_packaging/MyPrivateFeed/pypi/simple/"
```

Logs:
[log.txt](https://github.com/user-attachments/files/16231822/log.txt)
[log_succcess.txt](https://github.com/user-attachments/files/16231828/log_succcess.txt)

Looking at the log files, I could not spot an obvious reason for the failure.

---

_Comment by @fralik on 2024-07-16 15:03_

I identified the issue, which was neither with `uv` nor `pyspark`, but rather with Windows.

After spending an evening debugging, I discovered that the problem was due to Windows' long path limitation. When `uv` was installed from a private feed, the path to the package included more subfolders and characters compared to when it was downloaded via pip.

This issue arose because I was using conda to manage my Python distributions instead of downloading them natively from python.org. Installers from python.org include an option to enable long path support, but conda's installer does not have this option.

---

_Closed by @fralik on 2024-07-16 15:03_

---
