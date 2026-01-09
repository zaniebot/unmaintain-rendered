---
number: 6808
title: "`hello.py` does not pass `ruff format`"
type: issue
state: closed
author: my1e5
labels:
  - bug
assignees: []
created_at: 2024-08-29T12:34:11Z
updated_at: 2024-08-29T14:31:53Z
url: https://github.com/astral-sh/uv/issues/6808
synced_at: 2026-01-07T13:12:17-06:00
---

# `hello.py` does not pass `ruff format`

---

_Issue opened by @my1e5 on 2024-08-29 12:34_

uv version 0.4.0 creates a `hello.py` file by default when running
```
$ uv init my_project
```
The contents of `hello.py` are
```
$ cat hello.py
def main():
    print("Hello from my-project!")

if __name__ == "__main__":
    main()
```
Which is completely reasonable. But I noticed that this does not pass `ruff format`.
```
$ uv add ruff
$ uv run ruff format --diff .
--- hello.py
+++ hello.py
@@ -1,5 +1,6 @@
 def main():
     print("Hello from my-project!")

+
 if __name__ == "__main__":
     main()

1 file would be reformatted
```
Maybe you want `hello.py` to be ruff approved out the box? It's a minor thing though.

---

_Comment by @charliermarsh on 2024-08-29 13:00_

Lol, thank you. Yes. (PR welcome or we'll fix soon.)

---

_Label `bug` added by @charliermarsh on 2024-08-29 13:00_

---

_Referenced in [astral-sh/uv#6811](../../astral-sh/uv/pulls/6811.md) on 2024-08-29 13:06_

---

_Comment by @zanieb on 2024-08-29 13:37_

Thanks :) the problem with writing Python manually in a Rust string...

---

_Closed by @charliermarsh on 2024-08-29 14:31_

---
