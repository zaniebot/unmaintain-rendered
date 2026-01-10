---
number: 1416
title: Included requirements files should be relative to parent, not cwd
type: issue
state: closed
author: hauntsaninja
labels: []
assignees: []
created_at: 2024-02-16T02:48:43Z
updated_at: 2024-02-16T04:19:45Z
url: https://github.com/astral-sh/uv/issues/1416
synced_at: 2026-01-10T01:23:06Z
---

# Included requirements files should be relative to parent, not cwd

---

_Issue opened by @hauntsaninja on 2024-02-16 02:48_

```
. λ rm -rf tmp
. λ mkdir tmp
. λ cd tmp
./tmp λ printf 'mypy' > a.in                     
./tmp λ printf '-r a.in\nruff' > b.in
./tmp λ cd ..
. λ pip-compile --no-header --no-annotate tmp/b.in
WARNING: --strip-extras is becoming the default in version 8.0.0. To silence this warning, either use --strip-extras to opt into the new default or use --no-strip-extras to retain the existing behavior.
mypy==1.8.0
mypy-extensions==1.0.0
ruff==0.2.1
typing-extensions==4.9.0
. λ uv pip compile --no-header --no-annotate tmp/b.in
error: Error parsing included file in `tmp/b.in` at position 0
  Caused by: failed to open file `./a.in`
  Caused by: No such file or directory (os error 2)
```

---

_Comment by @charliermarsh on 2024-02-16 02:54_

I believe this is the same as https://github.com/astral-sh/uv/issues/1367.

---

_Comment by @charliermarsh on 2024-02-16 02:54_

Gonna look at it now.

---

_Comment by @hauntsaninja on 2024-02-16 02:55_

Thanks! I'll close this as a duplicate

---

_Closed by @hauntsaninja on 2024-02-16 02:55_

---

_Comment by @charliermarsh on 2024-02-16 02:56_

Do you know if pip has the same treatment for _local packages_, or is this just for requirements / constraints files?

---

_Comment by @charliermarsh on 2024-02-16 02:56_

Ahh, I see, it's just files (not directories): https://github.com/pypa/pip/issues/8765

---

_Comment by @hauntsaninja on 2024-02-16 03:01_

Looks like pip is inconsistent, in that in the following case it is relative to cwd:
```
λ ls tmp
a.in                         pypyp-1.1.0-py3-none-any.whl
λ cat tmp/a.in
pypyp-1.1.0-py3-none-any.whl
λ pip install -r tmp/a.in
WARNING: Requirement 'pypyp-1.1.0-py3-none-any.whl' looks like a filename, but the file does not exist
Processing ./pypyp-1.1.0-py3-none-any.whl (from -r tmp/a.in (line 1))
ERROR: Could not install packages due to an OSError: [Errno 2] No such file or directory: './pypyp-1.1.0-py3-none-any.whl'
λ cd tmp
./tmp master λ pip install -r a.in    
Processing ./pypyp-1.1.0-py3-none-any.whl (from -r a.in (line 1))
pypyp is already installed with the same version as the provided wheel. Use --force-reinstall to force an installation of the wheel.
```

I don't care about^ case at all

---

_Comment by @charliermarsh on 2024-02-16 03:02_

Okay yeah, makes sense.

---

_Referenced in [astral-sh/uv#1421](../../astral-sh/uv/pulls/1421.md) on 2024-02-16 03:57_

---

_Closed by @charliermarsh on 2024-02-16 04:19_

---
