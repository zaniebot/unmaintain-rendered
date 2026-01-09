---
number: 14839
title: "No checksum on \"uv self update\""
type: issue
state: open
author: jradxl
labels:
  - bug
assignees: []
created_at: 2025-07-23T08:06:03Z
updated_at: 2025-07-23T12:12:37Z
url: https://github.com/astral-sh/uv/issues/14839
synced_at: 2026-01-07T13:12:19-06:00
---

# No checksum on "uv self update"

---

_Issue opened by @jradxl on 2025-07-23 08:06_

### Summary

Is it to be expected that `uv self update` should work?

```
uv self update
info: Checking for updates...
error: The installation failed. Output from the installer: no checksums to verify
installing to /home/jradley/.local/bin

downloading uv 0.8.2 x86_64-unknown-linux-gnu
mv: cannot move '/tmp/tmp.UnPryJaZaV/uv' to '/home/jradley/.local/bin/uv': Text file busy
ERROR: command failed: mv /tmp/tmp.UnPryJaZaV/uv /home/jradley/.local/bin
 ~                                                                                                                                                                                                     | 08:59:40 
➜ curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.8.2/uv-installer.sh | sh
downloading uv 0.8.2 x86_64-unknown-linux-gnu
no checksums to verify
installing to /home/jradley/.local/bin
  uv
  uvx
everything's installed!
 ~                                                                                                                                                                                                     | 09:01:40 
➜ uv self update
info: Checking for updates...
success: You're on the latest version of uv (v0.8.2)
```

### Platform

24.04.2 LTS

### Version

8.0 -> 8.2

### Python version

python3 -V >> Python 3.12.3

---

_Label `bug` added by @jradxl on 2025-07-23 08:06_

---

_Comment by @zanieb on 2025-07-23 12:12_

Thanks for the report. cc @Gankra 

---
