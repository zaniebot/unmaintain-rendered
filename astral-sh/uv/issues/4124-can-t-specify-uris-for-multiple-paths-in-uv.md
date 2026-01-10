---
number: 4124
title: "Can't specify URIs (for multiple paths) in UV_CONSTRAINT"
type: issue
state: closed
author: henryiii
labels:
  - compatibility
assignees: []
created_at: 2024-06-07T03:44:08Z
updated_at: 2024-06-07T22:03:09Z
url: https://github.com/astral-sh/uv/issues/4124
synced_at: 2026-01-10T01:23:35Z
---

# Can't specify URIs (for multiple paths) in UV_CONSTRAINT

---

_Issue opened by @henryiii on 2024-06-07 03:44_

In pip, if you want to provide a path that has as space in it, you must provide a URI, as pip universally uses spaces for separating arguments. So for example, to set two constraint files that potentially contain spaces, you'd do:

```bash
echo "packaging==21.2" > 'file 1.txt'
echo "cowsay==6.0" > 'file 2.txt'
a = $(python3 -c 'import pathlib; print(pathlib.Path("file 1.txt").resolve().as_uri())')
b = $(python3 -c 'import pathlib; print(pathlib.Path("file 2.txt").resolve().as_uri())')
uv venv
PIP_CONSTRAINT="$a $b" pip install --python .venv cowsay packaging
```

(This is exactly what cibuildwheel does, it has it's own constraint file which it is combining with a user specified file(s), and it doesn't know if there might be spaces in the paths)

Trying this with uv just breaks on the URI with ``error: File not found: `file:///.../cibuildwheel/tmp/file%202.txt` ``. If I remove the spaces, it handles the space separated files just fine, but it doesn't seem to support URIs, which is the standard way to allow space separated paths in Pip.

---

_Referenced in [pypa/cibuildwheel#1856](../../pypa/cibuildwheel/pulls/1856.md) on 2024-06-07 03:48_

---

_Comment by @charliermarsh on 2024-06-07 03:51_

Sounds like a bug, thanks.

---

_Label `compatibility` added by @charliermarsh on 2024-06-07 03:51_

---

_Comment by @charliermarsh on 2024-06-07 03:52_

Does `PIP_CONSTRAINT="$a" pip install --python .venv cowsay packaging` work? (Is the "multiple files" relevant?)

---

_Comment by @henryiii on 2024-06-07 04:02_

Multiple files is not relevant actually, other than being the reason that spaces aren't available for file paths. Pip generally accepts a URI anywhere it accepts a file path, which happens to be important for environment variables that use the space for other purposes.

Yes, pip works with one file.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-07 15:08_

---

_Referenced in [astral-sh/uv#4145](../../astral-sh/uv/pulls/4145.md) on 2024-06-07 20:29_

---

_Closed by @charliermarsh on 2024-06-07 22:03_

---
