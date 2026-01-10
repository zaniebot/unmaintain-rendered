```yaml
number: 3203
title: "Undefined name: `ExceptionGroup`"
type: issue
state: closed
author: torarvid
labels: []
assignees: []
created_at: 2023-02-24T08:16:14Z
updated_at: 2023-02-24T12:10:57Z
url: https://github.com/astral-sh/ruff/issues/3203
synced_at: 2026-01-10T11:09:46Z
```

# Undefined name: `ExceptionGroup`

---

_Issue opened by @torarvid on 2023-02-24 08:16_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hello there 👋🏻, and thanks again for this wonderful tool 🎉

I'm unsure whether this belongs here or in RustPython, so feel free to redirect me there 😉 

Ruff version 0.0.252
Python version 3.11.1

I created this small test.py file:
```python
eg: ExceptionGroup = ExceptionGroup("test", [Exception("test1"), Exception("test2")])
print(eg)
```
and `ruff test.py` yields
```bash
test.py:1:5: F821 Undefined name `ExceptionGroup`
test.py:1:22: F821 Undefined name `ExceptionGroup`
Found 2 errors.
```
As far as I can understand, `ExceptionGroup` is a Python builtin (new in 3.11)

---

_Comment by @torarvid on 2023-02-24 08:21_

Ah! I tried compiling RustPython now, and I guess this result means the bug belongs there:
```bash
❯ rustpython test.py 
Traceback (most recent call last):
  File "test.py", line 1, in <module>
    eg: ExceptionGroup = ExceptionGroup("test", [Exception("test1"), Exception("test2")])
NameError: name 'ExceptionGroup' is not defined
```

---

_Comment by @torarvid on 2023-02-24 10:23_

Confirmed in https://github.com/RustPython/RustPython/issues/4563 that it belongs there, so I'll close this issue. Sorry for the noise.

---

_Closed by @torarvid on 2023-02-24 10:23_

---

_Comment by @charliermarsh on 2023-02-24 12:10_

All good! This actually _is_ a bug in Ruff but it’s already been fixed and will go out in the next release. (I added support for exception groups to the RustPython _parser_, but not the runtime, which we don’t rely on. Handling the ExceptionGroup built in is Ruff’s responsibility.)

---
