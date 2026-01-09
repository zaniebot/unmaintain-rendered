---
number: 11878
title: uv pip install on a venv creates a .lock file
type: issue
state: closed
author: code-simp
labels:
  - question
assignees: []
created_at: 2025-03-01T06:29:40Z
updated_at: 2025-03-14T00:38:29Z
url: https://github.com/astral-sh/uv/issues/11878
synced_at: 2026-01-07T13:12:18-06:00
---

# uv pip install on a venv creates a .lock file

---

_Issue opened by @code-simp on 2025-03-01 06:29_

### Question

Installing python packages using uv creates a .lock file inside the venv directory
wondering if I'm missing something or uv does not cleanup the .lock file post installation

Which leads to: 
Intermittent OS Error 37 "No locks available" on a machine shared by our team's developers

Steps to reproduce:
```
uv venv test_venv
source test_venv/bin/activate
uv pip install <any-library>
ls -ltra test_venv | grep lock
```

my output on a mac m1 (OS 13):
ls -ltra | grep lock -> 
-rwxrwxrwx   1 xxx  yyy  0 Mar  1 11:42 .lock

### Platform

macOS 13 ARM64

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

---

_Label `question` added by @code-simp on 2025-03-01 06:29_

---

_Comment by @charliermarsh on 2025-03-01 14:34_

This is intentional. These are synchronization locks. Removing them within uv is actually highly non-trivial without introducing race conditions since other processes could be waiting to acquire them, etc. I’ve never heard of this error before though (and we’ve used these since uv first came out, more or less). Are you able to put together a complete reproduction?

---

_Comment by @code-simp on 2025-03-01 14:57_

Thanks for the reply

not sure if I can reproduce the OS 37 error intentionally, as the occurrence itself seems to be random across devs. But will give it a try.

but most likely would end up writing a post script to remove the lock file, unless its suggested not to.

---

_Comment by @charliermarsh on 2025-03-02 03:14_

You can remove it post-install, if you're sure that nothing is operating on the environment. (These locks exist to ensure that if you run `uv sync` or similar from multiple processes at once, they don't clobber each other.)

---

_Closed by @charliermarsh on 2025-03-14 00:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-14 00:38_

---
