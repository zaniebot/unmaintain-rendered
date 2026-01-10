---
number: 1344
title: "`uv pip compile --python-version ...` breaks when pyenv is used"
type: issue
state: closed
author: ThiefMaster
labels:
  - bug
assignees: []
created_at: 2024-02-15T21:06:11Z
updated_at: 2025-01-07T19:52:13Z
url: https://github.com/astral-sh/uv/issues/1344
synced_at: 2026-01-10T01:23:05Z
---

# `uv pip compile --python-version ...` breaks when pyenv is used

---

_Issue opened by @ThiefMaster on 2024-02-15 21:06_

```
$ uv pip compile --python-version 3.9 requirements.in -o requirements.txt
error: Querying Python at `/home/adrian/.pyenv/shims/python3.9` failed with status exit status: 127:
--- stdout:

--- stderr:
pyenv: python3.9: command not found

The `python3.9' command exists in these Python versions:
  [...]

Note: See 'pyenv help global' for tips on allowing both
      python2 and python3 to be found.
---
```

---

_Comment by @zanieb on 2024-02-15 21:07_

Thank you! This is a problem with the shim unfortunately. We can fix this but it probably won't be today.

---

_Label `bug` added by @zanieb on 2024-02-15 21:07_

---

_Comment by @brainhart on 2024-02-23 20:14_

@ThiefMaster I hit this as well and realized that this could be fixed by adjusting my pyenv configuration!

With pyenv you are able to activate multiple versions at the same time. See below for text in the [pyenv README](https://github.com/pyenv/pyenv?tab=readme-ov-file#understanding-shims)
> NOTE: You can activate multiple versions at the same time, including multiple versions of Python2 or Python3 simultaneously. This allows for parallel usage of Python2 and Python3, and is required with tools like tox. For example, to instruct Pyenv to first use your system Python and Python3 (which are e.g. 2.7.9 and 3.4.2) but also have Python 3.3.6, 3.2.1, and 2.5.2 available, you first pyenv install the missing versions, then set pyenv global system 3.3.6 3.2.1 2.5.2. Then you'll be able to invoke any of those versions with an appropriate pythonX or pythonX.Y name.

Based on the above, I ran the following so that all my pyenv versions are available in global.
```
pyenv versions --bare --skip-aliases --skip-envs | xargs pyenv global
```

This ensured that all the versions are available for use in my shell:
```
❯ pyenv versions
  system
* 3.8.18 (set by /Users/brian/.local/share/pyenv/version)
* 3.11.6 (set by /Users/brian/.local/share/pyenv/version)

❯ python3.8 --version
Python 3.8.18

❯ python3.11 --version
Python 3.11.6
```

And lastly I confirmed that this was successfully usable by `uv venv` (I also tested with `uv pip compile -p`):
```
❯ uv venv -p 3.8
Using Python 3.8.18 interpreter at /Users/brian/.local/share/pyenv/versions/3.8.18/bin/python3.8
Creating virtualenv at: .venv

❯ uv venv -p 3.11
Using Python 3.11.6 interpreter at /Users/brian/.local/share/pyenv/versions/3.11.6/bin/python3.11
Creating virtualenv at: .venv
```

---

_Comment by @zanieb on 2025-01-07 19:52_

I think we ignore these now.

---

_Closed by @zanieb on 2025-01-07 19:52_

---
