---
number: 2017
title: uv pip compile sometimes outputs ansi escapes in file
type: issue
state: closed
author: bluss
labels:
  - bug
  - cli
assignees: []
created_at: 2024-02-27T15:16:30Z
updated_at: 2024-02-27T16:02:24Z
url: https://github.com/astral-sh/uv/issues/2017
synced_at: 2026-01-10T01:23:10Z
---

# uv pip compile sometimes outputs ansi escapes in file

---

_Issue opened by @bluss on 2024-02-27 15:16_

Experienced behaviour: Can't call "rye add" or "rye sync" when uv is enabled in rye, from a jupyterlab python kernel command line.

I think this could be a bug in uv, that's why I'm reporting it here. This is how I reproduce it:

1. Using jupyterlab (v3.6 or v4.1 both show this problem), create a new notebook in a new directory.
2. Write 'numpy>=1.2' to 'requirements.txt'
3. Run `!uv pip compile -o "outputfile" "requirements.txt"`  in a python notebook
4. Bug: outputfile contains ansi escapes in the lockfile output.

For some reason this happens in the notebook environment but not in a regular terminal. The original bug in rye seems to be related to this - it also gets this behaviour when called from a notebook. Rye could work around it using --no-color, but it might be something that can be fixed in uv.

When using rye in the notebook the output looks something like this:

```
!rye add numpy
```
```
Added numpy>=1.26.4 as regular dependency
Reusing already existing virtualenv
Generating production lockfile: /.../newjupyterlabproject/requirements.lock
error: Unexpected '', expected '-c', '-e', '-r' or the start of a requirement in `/tmp/.tmpXbPWlj/requirements.txt` at position 221
error: could not write production lockfile for project

Caused by:
    failed to generate lockfile
```

uv 0.1.11
Linux

---

_Comment by @trag1c on 2024-02-27 15:25_

It also affects VSCode notebooks 👍 

---

_Label `bug` added by @charliermarsh on 2024-02-27 15:25_

---

_Comment by @charliermarsh on 2024-02-27 15:25_

Thanks!

---

_Label `cli` added by @charliermarsh on 2024-02-27 15:31_

---

_Assigned to @konstin by @konstin on 2024-02-27 15:36_

---

_Referenced in [astral-sh/uv#2018](../../astral-sh/uv/pulls/2018.md) on 2024-02-27 15:45_

---

_Closed by @konstin on 2024-02-27 16:02_

---

_Referenced in [tox-dev/tox-uv#38](../../tox-dev/tox-uv/issues/38.md) on 2024-03-14 20:09_

---
