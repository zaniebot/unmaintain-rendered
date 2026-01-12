```yaml
number: 7994
title: Inconsistent EXE001 behavior
type: issue
state: closed
author: Scrat94
labels:
  - question
assignees: []
created_at: 2023-10-16T20:17:28Z
updated_at: 2023-10-16T21:20:17Z
url: https://github.com/astral-sh/ruff/issues/7994
synced_at: 2026-01-12T15:54:47Z
```

# Inconsistent EXE001 behavior

---

_@Scrat94_

Hello,

Steps to reproduce:
1. Have file `main.py`
2. On the first line:  `#!/usr/bin/env python3`
3. Run ruff locally on windows
4. Run ruff on Linux in GitLab CI runner

Current behavior:
* Locally on windows no finding/error occurs
* On GitLab Runner (Linux) the following error shows up: `main.py:1:1: EXE001 Shebang is present but file is not executable`

Expected behavior:
* Same result on local (windows) as well as on GitLab CI (Linux)

Is my understanding correct, or is this the intended behavior? Much thanks in advance!

---

_Comment by @charliermarsh on 2023-10-16 20:41_

So, we don't enforce that rule on Windows at all, since IIUC there is no concept of file modes on Windows. So this may be expected, in that if the file isn't marked as executable, you wouldn't see an error on Windows, but _would_ on Linux.


---

_Comment by @charliermarsh on 2023-10-16 20:41_

I'm wondering if you can run `git update-index --chmod=+x /path/to/exe`?

---

_Comment by @Scrat94 on 2023-10-16 21:08_

> So, we don't enforce that rule on Windows at all, since IIUC there is no concept of file modes on Windows. So this may be expected, in that if the file isn't marked as executable, you wouldn't see an error on Windows, but _would_ on Linux.

Thanks for the explanation, I guess I will just ignore the EXE-Rule(s). Currently cannot run the `git update-index --chmod=+x /path/to/exe`

We can close the issue. Thanks for the quick reply.

---

_Closed by @Scrat94 on 2023-10-16 21:08_

---

_Comment by @charliermarsh on 2023-10-16 21:20_

No worries -- sorry for the confusion.

---

_Label `question` added by @charliermarsh on 2023-10-16 21:20_

---
