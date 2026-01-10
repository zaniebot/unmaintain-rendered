---
number: 13885
title: Unable to install
type: issue
state: closed
author: Konev-359
labels:
  - question
assignees: []
created_at: 2025-06-06T13:08:37Z
updated_at: 2025-06-06T13:56:53Z
url: https://github.com/astral-sh/uv/issues/13885
synced_at: 2026-01-10T01:25:39Z
---

# Unable to install

---

_Issue opened by @Konev-359 on 2025-06-06 13:08_

### Summary

Hello,
I'm having difficulty to install uv with the default method on linux :

>~ curl -LsSf https://astral.sh/uv/0.7.11/install.sh | sh
downloading uv 0.7.11 x86_64-unknown-linux-gnu
curl: (23) client returned ERROR on write of 16375 bytes
failed to download https://github.com/astral-sh/uv/releases/download/0.7.11/uv-x86_64-unknown-linux-gnu.tar.gz
this may be a standard network error, but it may also indicate
that uv's release process is not working. When in doubt
please feel free to open an issue!

### Platform

Ubuntu 24.04.2 LTS

### Version

0.7.11

### Python version

3.12.3

---

_Label `bug` added by @Konev-359 on 2025-06-06 13:08_

---

_Comment by @zanieb on 2025-06-06 13:36_

This doesn't look like a uv-specific problem, can you `curl` that URL separately? Is this spurious or does it always fail?

---

_Label `bug` removed by @zanieb on 2025-06-06 13:36_

---

_Label `question` added by @zanieb on 2025-06-06 13:36_

---

_Comment by @Konev-359 on 2025-06-06 13:49_

It always fails, I downloaded the installer separately :
> ~ ./uv-installer.sh         
downloading uv 0.7.11 x86_64-unknown-linux-gnu
curl: (23) client returned ERROR on write of 1369 bytes
failed to download https://github.com/astral-sh/uv/releases/download/0.7.11/uv-x86_64-unknown-linux-gnu.tar.gz
this may be a standard network error, but it may also indicate
that uv's release process is not working. When in doubt
please feel free to open an issue! 

---

_Comment by @zanieb on 2025-06-06 13:51_

Can you do, e.g., `wget https://github.com/astral-sh/uv/releases/download/0.7.11/uv-x86_64-unknown-linux-gnu.tar.gz`?

---

_Comment by @zanieb on 2025-06-06 13:52_

Do you have a firewall? Do you have disk space available?

---

_Comment by @Konev-359 on 2025-06-06 13:55_

It succeeded to download the archive, thank you ! Have a great day :)

---

_Comment by @zanieb on 2025-06-06 13:56_

Okay, thanks!

---

_Closed by @zanieb on 2025-06-06 13:56_

---
