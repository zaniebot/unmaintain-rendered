---
number: 7197
title: "`dbm.gnu` not available"
type: issue
state: closed
author: suomela
labels:
  - external
assignees: []
created_at: 2024-09-08T19:41:11Z
updated_at: 2025-07-17T12:28:11Z
url: https://github.com/astral-sh/uv/issues/7197
synced_at: 2026-01-10T01:24:12Z
---

# `dbm.gnu` not available

---

_Issue opened by @suomela on 2024-09-08 19:41_

My system Python defaults to using `dbm.gnu` when I use the `dbm` module (macOS, Homebrew, installed `python@3.12` and `python-gdbm@3.12`.

However, Python from `uv` apparently does not provide `dbm.gnu`, and hence cannot open files created with my system Python.

Steps to reproduce:
```
brew install python python-gdbm
uv init --python-preference=only-managed foo
cd foo
python3 -c 'import dbm; dbm.open("test", "c")'
python3 -c 'import dbm; dbm.open("test", "r")'
uv run python -c 'import dbm; dbm.open("test", "r")'
```

The final command fails with:
```
Using Python 3.12.5
Creating virtualenv at: .venv
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/.../.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/lib/python3.12/dbm/__init__.py", line 91, in open
    raise error[0]("db type is {0}, but the module is not "
dbm.error: db type is dbm.gnu, but the module is not available
```

It is not clear to me how do I tell `uv` that I would like to get a Python that supports `dbm.gnu`.

---

_Comment by @apollo13 on 2024-09-09 06:18_

Probably related to this: https://gregoryszorc.com/docs/python-build-standalone/main/technotes.html?highlight=dbm#dbm -- if yes there isn't much uv can do. You can either build your own managed python (not sure if you can use that with uv though) or use the system python.

---

_Label `upstream` added by @charliermarsh on 2024-09-16 02:52_

---

_Comment by @ikks on 2024-10-30 20:31_

I do not understand clearly if uv depends on python-build-standalone, if this is the case, uv can do nothing about this issue, in other case, is it possible that uv can enable the gdbm module to be available?  I would love to continue using uv in cases that gdbm is required.

---

_Comment by @f-reinsch on 2025-07-16 19:00_

Hi all,

is there any news on this?
I encountered the same issue today while updating one of my projects to python 3.13.5. 
I get this error only because of celery, which is kind of dependent on dbm.gnu at least for the state db´s it creates for the worker and beat processes.

My current workaround would be to also have to install pyenv to install the working python interpreter and let uv use this as the base intepreter.

---

_Comment by @zanieb on 2025-07-17 01:06_

We do maintain `python-build-standalone`, and use it for our managed Python distributions.

I don't know what's to be done about the `dbm.gnu` problem though. As described in the link, it's not included because it's GPL licensed and we avoid such dependencies in the `python-build-standalone`.

The easiest way to work around this with uv is to set `--no-managed-python` or `UV_PYTHON_PREFERENCE=system`

---

_Comment by @zanieb on 2025-07-17 01:14_

Can you share more about why / how Celery depends specifically on `dbm.gnu`? It looks like that's only the case if an existing state file created by a different Python version is being read? If you're using a consistent Python distribution, i.e., uv's, then it should be self-compatible?

---

_Comment by @f-reinsch on 2025-07-17 12:00_

Hi @zanieb,

thank you for your input. I just double checked if your approach by deleting and recreating the state-db works with an uv managed python installation and voila it works! Insane support, love to see it!!

Keep up the great work you do at astral by revolutionizing the python ecosystem 👍🏻 

---

_Comment by @zanieb on 2025-07-17 12:28_

I'm going to close this as this is as-designed, but feel free to comment if you have a use-case that this blocks!

---

_Closed by @zanieb on 2025-07-17 12:28_

---
