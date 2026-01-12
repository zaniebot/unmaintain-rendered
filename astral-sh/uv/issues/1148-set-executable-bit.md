```yaml
number: 1148
title: Set executable bit
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-01-27T10:47:16Z
updated_at: 2024-01-28T00:22:45Z
url: https://github.com/astral-sh/uv/issues/1148
synced_at: 2026-01-12T15:58:25Z
```

# Set executable bit

---

_@konstin_

The command `python -m ziglang` works after `pip install ziglang`, but fails with `puffin pip install ziglang` ("PermissionError: [Errno 13] Permission denied") because we're not setting the executable bit for the zig executable:

**pip**

```
ls -lah .venv/lib/python3.12/site-packages/ziglang
total 142M
drwxrwxr-x  5 konsti konsti 4,0K Jan 27 11:44 .
drwxrwxr-x  7 konsti konsti 4,0K Jan 27 11:44 ..
drwxrwxr-x  3 konsti konsti 4,0K Jan 27 11:44 doc
-rw-rw-r--  1 konsti konsti    0 Jan 27 11:44 __init__.py
drwxrwxr-x 13 konsti konsti 4,0K Jan 27 11:44 lib
-rw-rw-r--  1 konsti konsti 1,1K Jan 27 11:44 LICENSE
-rw-rw-r--  1 konsti konsti  198 Jan 27 11:44 __main__.py
drwxrwxr-x  2 konsti konsti 4,0K Jan 27 11:44 __pycache__
-rw-rw-r--  1 konsti konsti 4,0K Jan 27 11:44 README.md
-rwxrwxr-x  1 konsti konsti 142M Jan 27 11:44 zig
```

**puffin**

```
$ ls -lah .venv/lib/python3.12/site-packages/ziglang
total 142M
drwxrwxr-x  5 konsti konsti 4,0K Jan 27 11:43 .
drwxrwxr-x  7 konsti konsti 4,0K Jan 27 11:43 ..
drwxrwxr-x  3 konsti konsti 4,0K Jan 27 11:43 doc
-rw-rw-r--  2 konsti konsti    0 Jan 27 11:42 __init__.py
drwxrwxr-x 13 konsti konsti 4,0K Jan 27 11:43 lib
-rw-rw-r--  2 konsti konsti 1,1K Jan 27 11:42 LICENSE
-rw-rw-r--  2 konsti konsti  198 Jan 27 11:42 __main__.py
drwxrwxr-x  2 konsti konsti 4,0K Jan 27 11:43 __pycache__
-rw-rw-r--  2 konsti konsti 4,0K Jan 27 11:42 README.md
-rw-rw-r--  2 konsti konsti 142M Jan 27 11:42 zig
```

To do: Investigate for which files we need to set the executable bit

---

_Label `bug` added by @konstin on 2024-01-27 10:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-27 11:23_

---

_Comment by @charliermarsh on 2024-01-27 11:23_

I can take a look at this.

---

_Comment by @charliermarsh on 2024-01-27 11:37_

Oh, I think it's not being set during _download_.

---

_Comment by @charliermarsh on 2024-01-27 11:38_

That makes sense. It's a regression from when we changed to streaming-unzip. Good catch!

---

_Comment by @charliermarsh on 2024-01-27 11:47_

I'll add a test too. Though not Zig becomes the binary is insanely large lmao.

---

_Comment by @charliermarsh on 2024-01-27 12:34_

Okay actually not trivial because the executable bits are stored in the central directory archive and not as part of the entry. Looking into it.

---

_Closed by @charliermarsh on 2024-01-28 00:22_

---
