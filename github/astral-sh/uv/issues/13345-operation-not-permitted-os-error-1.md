---
number: 13345
title: Operation not permitted (os error 1)
type: issue
state: closed
author: Cheiklf
labels:
  - bug
assignees: []
created_at: 2025-05-08T11:17:22Z
updated_at: 2025-05-09T09:37:42Z
url: https://github.com/astral-sh/uv/issues/13345
synced_at: 2026-01-07T13:12:18-06:00
---

# Operation not permitted (os error 1)

---

_Issue opened by @Cheiklf on 2025-05-08 11:17_

### Summary

Hello,
UV works well on my "root @ bureau-pro in ~" partition but fails to create an environment on a different disk. 
" root @ bureau-pro in /mnt/h/formation_data/" This is a disk where my Python projects are installed.
Still a to symlink problem.

`
error: failed to symlink file from /root/.local/share/uv/python/cpython-3.13.3-linux-x86_64-gnu/bin/python3.13 to /mnt/h/formation_data/projet_de_formation/projet_UV/test3/.venv/bin/python: Operation not permitted (os error 1)
`



![Image](https://github.com/user-attachments/assets/f5f1de25-0541-4c69-9992-8c1937c14b73)

### Platform

WSL2  Ubuntu 24.04.1 LTS

### Version

7.3

### Python version

Python 3.13

---

_Label `bug` added by @Cheiklf on 2025-05-08 11:17_

---

_Comment by @konstin on 2025-05-08 12:02_

What filesystem are you using on the `/mnt` disk, does it support symlinks? It's generally possible to create cross-disk symlinks (as opposed to hardlinks), so I suspect that something else is the problem here.

---

_Comment by @Cheiklf on 2025-05-09 09:37_

Hello,
You are right, the problem come from the format. It is exFAT and it's not compatible whith symlinks.
I've format my disk with NTFS and evrey thing is ok
think again for your help

---

_Closed by @Cheiklf on 2025-05-09 09:37_

---
