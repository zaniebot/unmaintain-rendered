```yaml
number: 5543
title: uv 0.2.30 breaks editable projects when installed a second or more times due to caching
type: issue
state: closed
author: bulletmark
labels:
  - bug
assignees: []
created_at: 2024-07-29T01:50:18Z
updated_at: 2024-07-29T02:39:49Z
url: https://github.com/astral-sh/uv/issues/5543
synced_at: 2026-01-12T15:58:56Z
```

# uv 0.2.30 breaks editable projects when installed a second or more times due to caching

---

_@bulletmark_

uv 0.2.30 breaks editable projects due to something relating to the cache. This problem does not occur with uv 0.2.29 and earlier versions. The annotated command steps below best demonstrate the issue:

```sh
# Show uv version and platform
$ uv --version
uv 0.2.30
$ uname -a
Linux lt 6.10.1-arch1-1 #1 SMP PREEMPT_DYNAMIC Wed, 24 Jul 2024 22:25:43 +0000 x86_64 GNU/Linux

# This problem happens with all my own projects but let's use a generic project to demonstrate the issue
$ git clone https://github.com/VaasuDevanS/cowsay-python
Cloning into 'cowsay-python'...
remote: Enumerating objects: 473, done.
remote: Counting objects: 100% (192/192), done.
remote: Compressing objects: 100% (89/89), done.
remote: Total 473 (delta 108), reused 159 (delta 91), pack-reused 281
Receiving objects: 100% (473/473), 277.85 KiB | 4.34 MiB/s, done.
Resolving deltas: 100% (239/239), done.

# Create venv
$ cd cowsay-python
$ uv venv
Using Python 3.12.4 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

# Install this project (1st time)
$ uv pip install -e .
Resolved 1 package in 3ms
   Built cowsay @ file:///home/mark/cowsay-python
Prepared 1 package in 923ms
Installed 1 package in 0.60ms
 + cowsay==6.1 (from file:///home/mark/cowsay-python)

# List the project and see it is correctly shown as editable
$ uv pip list -v
DEBUG uv 0.2.30
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.12.4-linux-x86_64-gnu` at `/home/mark/cowsay-python/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.4 environment at .venv/bin/python3
Package Version Editable project location
------- ------- -------------------------
cowsay  6.1     /home/mark/cowsay-python

# Remove the venv and create it again.
$ rm -rf .venv
$ uv venv
Using Python 3.12.4 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

# Install this project (2nd time)
$ uv pip install -e .
Resolved 1 package in 4ms
Installed 1 package in 3ms
 + cowsay==6.1 (from file:///home/mark/cowsay-python)

# List the project and see it is incorrectly not shown as editable
$ uv pip list -v
DEBUG uv 0.2.30
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.12.4-linux-x86_64-gnu` at `/home/mark/cowsay-python/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.4 environment at .venv/bin/python3
Package Version
------- -------
cowsay  6.1

# Remove the UV cache because it seems related to this bug.
$ rm -rf ~/.cache/uv

# Remove the venv and create it again.
$ rm -rf .venv
$ uv venv
Using Python 3.12.4 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

# Install this project (3rd time)
$ uv pip install -e .
Resolved 1 package in 2ms
   Built cowsay @ file:///home/mark/cowsay-python
Prepared 1 package in 835ms
Installed 1 package in 0.50ms
 + cowsay==6.1 (from file:///home/mark/cowsay-python)

# List the project and see it is again correctly shown as editable
$ uv pip list -v
DEBUG uv 0.2.30
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.12.4-linux-x86_64-gnu` at `/home/mark/cowsay-python/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.4 environment at .venv/bin/python3
Package Version Editable project location
------- ------- -------------------------
cowsay  6.1     /home/mark/cowsay-python
```


---

_Comment by @charliermarsh on 2024-07-29 02:22_

Interesting, thank you. I think we aren't differentiating between editable and non-editable in the cache. I will fix tomorrow.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-29 02:22_

---

_Label `bug` added by @charliermarsh on 2024-07-29 02:22_

---

_Comment by @charliermarsh on 2024-07-29 02:23_

The code actually looks like it handles this correctly, hmm.

---

_Comment by @charliermarsh on 2024-07-29 02:26_

Ok, I see the problem. Sorry about that.

---

_Comment by @charliermarsh on 2024-07-29 02:29_

I think the contents of `site-packages` are actually correct, it's just that the `direct-url.json` is not, so `pip list` is reporting that the projects aren't editable when they are. Are you seeing some other issue beyond that?

---

_Comment by @bulletmark on 2024-07-29 02:32_

That is correct. It is only the list command reporting that the project is not editable. My [installer tool](https://github.com/bulletmark/pipxu) parses `list` output to determine the editable project dir so this problem is very apparent to me.

---

_Comment by @charliermarsh on 2024-07-29 02:32_

Ok thanks. I've fixed it in #5545.

---

_Closed by @charliermarsh on 2024-07-29 02:39_

---
