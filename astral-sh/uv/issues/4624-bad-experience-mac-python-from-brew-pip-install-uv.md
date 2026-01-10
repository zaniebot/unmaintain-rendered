---
number: 4624
title: "Bad experience: Mac, python from brew, pip install uv"
type: issue
state: closed
author: anki-code
labels:
  - question
assignees: []
created_at: 2024-06-28T15:45:22Z
updated_at: 2024-06-28T15:56:24Z
url: https://github.com/astral-sh/uv/issues/4624
synced_at: 2026-01-10T01:23:39Z
---

# Bad experience: Mac, python from brew, pip install uv

---

_Issue opened by @anki-code on 2024-06-28 15:45_

Hey! Thank you for this awesome thing!

I tried to install it but had bad experience on Mac. Let's take a look:

```xsh
which pip
# /opt/homebrew/bin/pip

pip -V
# pip 22.3.1 from /usr/local/lib/rustpython3.11/site-packages/pip

pip install uv
# ERROR: Could not install packages due to an 
# OSError: [Errno 13] Permission denied (os error 13): '/usr/local/bin/uv' -> 'None' 🔴 
# Consider using the `--user` option or check the permissions.

pip install --user uv
# Successfully installed uv-0.2.17
# WARNING: There was an error checking the latest version of pip. 🔴 

uv
# The command line interface for the uv binary.

uv pip install pandas
# error: No Python interpreters found in system toolchains 🔴 
```


---

_Comment by @charliermarsh on 2024-06-28 15:46_

Do you have Python installed?

---

_Renamed from "Bad experience on Mac with python from brew" to "Bad experience: Mac, python from brew, pip install uv" by @anki-code on 2024-06-28 15:46_

---

_Comment by @anki-code on 2024-06-28 15:48_

```xsh
which python
# /opt/homebrew/bin/python
```

---

_Comment by @charliermarsh on 2024-06-28 15:49_

You should run `uv venv` first to create a virtual environment. (That error message is fixed on `main`.)

---

_Comment by @zanieb on 2024-06-28 15:49_

I believe I just fixed the error message you're seeing at https://github.com/astral-sh/uv/pull/4596

We require creation of a virtual environment, so you need to run `uv venv` before you can `uv pip install`.

We don't support pip's user flag (#2077 )

---

_Label `question` added by @zanieb on 2024-06-28 15:49_

---

_Comment by @anki-code on 2024-06-28 15:54_

I can't see the future error message in 4596 but I believe it will be like "Activate virtual env before install packages."

I used `--user` because of `OSError: [Errno 13] Permission denied (os error 13): '/usr/local/bin/uv' -> 'None'`.

After activating the environment it works as expected:
```xsh
execx($(micromamba shell hook --shell xonsh))
mamba activate @('myenv')

uv pip install pandas
# Resolved 6 packages in 1.43s
# Prepared 5 packages in 2.66s
# Installed 5 packages in 75ms
```
Thanks!

---

_Closed by @anki-code on 2024-06-28 15:55_

---

_Comment by @zanieb on 2024-06-28 15:56_

Glad it's sorted out. We'll add more suggestions to the error messages there soon :)

---
