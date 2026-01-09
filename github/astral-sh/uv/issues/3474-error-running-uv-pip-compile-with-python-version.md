---
number: 3474
title: "Error running uv pip compile with `--python-version` specified when Python 2 is on the PATH"
type: issue
state: closed
author: notatallshaw-gts
labels:
  - bug
assignees: []
created_at: 2024-05-08T22:30:13Z
updated_at: 2024-05-09T14:45:05Z
url: https://github.com/astral-sh/uv/issues/3474
synced_at: 2026-01-07T13:12:17-06:00
---

# Error running uv pip compile with `--python-version` specified when Python 2 is on the PATH

---

_Issue opened by @notatallshaw-gts on 2024-05-08 22:30_

I am looking to migrate some scripts I have at work to use uv, perhaps I am using the `--python-version` flag wrong, does a Python with that version need to be available on the PATH or can uv resolve without it?

```
$ uv -V
uv 0.1.41

$ python -V
Python 3.11.4

$ echo "requests" > test.txt

$ uv pip compile --python-version 3.10.6  test.txt 
error: Querying Python at `/usr/bin/python` failed with status exit status: 2 with exit status: 2
--- stdout:

--- stderr:
Unknown option: -I
usage: /usr/bin/python [option] ... [-c cmd | -m mod | file | -] [arg] ...
Try `python -h' for more information.
---

$ /usr/bin/python -V
Python 2.7.15
```

---

_Label `bug` added by @charliermarsh on 2024-05-08 22:31_

---

_Comment by @charliermarsh on 2024-05-08 22:40_

I can fix that. We're not catching that specific failure (`-I` is unsupported).

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-08 22:40_

---

_Comment by @notatallshaw-gts on 2024-05-08 22:41_

I don't know if it makes a difference, but the reproduction is using a conda environment.

I have a couple of scripts that do something very similar to uv pip compile and sync by using a mix of conda and pip, I am experimenting if I can significantly simplify and speed them up with uv. 

---

_Referenced in [astral-sh/uv#3476](../../astral-sh/uv/pulls/3476.md) on 2024-05-09 00:52_

---

_Closed by @charliermarsh on 2024-05-09 03:25_

---

_Comment by @notatallshaw on 2024-05-09 14:19_

Thanks as always for the lighting quick fix! 

My aging work environment always seems to have a way to find interesting issues. I'll continue working to see if I can replace large parts of my logic with uv and if it finds anymore issues 🙂 .

---

_Comment by @charliermarsh on 2024-05-09 14:45_

No prob, let me know if you continue to see Python 2 issues. It's tricky because we can't actually get a structured error here (we're passing `-I` to the interpreter which isn't supported prior to 3.4).

---

_Referenced in [astral-sh/uv#3579](../../astral-sh/uv/issues/3579.md) on 2024-05-14 14:28_

---
