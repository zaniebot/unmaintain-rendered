---
number: 7152
title: "`uv run --with` fails to include virtual environment in `sys.path` for Homebrew-installed Python"
type: issue
state: closed
author: dhduvall
labels: []
assignees: []
created_at: 2024-09-07T02:05:08Z
updated_at: 2024-09-09T13:19:56Z
url: https://github.com/astral-sh/uv/issues/7152
synced_at: 2026-01-10T01:24:11Z
---

# `uv run --with` fails to include virtual environment in `sys.path` for Homebrew-installed Python

---

_Issue opened by @dhduvall on 2024-09-07 02:05_

I'm trying to run `uv run --with pylint -- pylint ...`, and `pylint` is complaining about not being able to resolve `import` statements in my source. Indeed, `sys.path` in that context doesn't have the virtual environment's packages.

```toml
[project]
name = "test"
version = "1.2.3"
requires-python = "==3.11.*"
dependencies = [
  "regex", # or whatever
]
```

```console
$ uv run --with pylint -- python -c 'import sys; print("\n".join(sys.path))'
Using Python 3.11.9 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
Removed virtual environment at: .venv
Creating virtualenv at: .venv
Installed 1 package in 4ms

/opt/homebrew/Cellar/python@3.11/3.11.9_1/Frameworks/Python.framework/Versions/3.11/lib/python311.zip
/opt/homebrew/Cellar/python@3.11/3.11.9_1/Frameworks/Python.framework/Versions/3.11/lib/python3.11
/opt/homebrew/Cellar/python@3.11/3.11.9_1/Frameworks/Python.framework/Versions/3.11/lib/python3.11/lib-dynload
/Users/duvall/Library/Caches/uv/archive-v0/rpeJdmPWBfImmHdlLv7hN/lib/python3.11/site-packages
$ uv run --with pylint -- python -c "import regex"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'regex'
```

Interestingly, this works fine with the Python 3.9 installed on the system, but not on any version installed by Homebrew (3.9 included):
```console
❯ uv run --with pylint python -c 'import sys; print("\n".join(sys.path))'
Using Python 3.9.6 interpreter at: /Library/Developer/CommandLineTools/usr/bin/python3
Removed virtual environment at: .venv
Creating virtualenv at: .venv
Installed 1 package in 2ms

/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python39.zip
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/lib-dynload
/Users/duvall/Library/Caches/uv/archive-v0/AiY6Z--H2Ja4w3r7Vaquv/lib/python3.9/site-packages
/Users/duvall/test/.venv/lib/python3.9/site-packages
```

```console
$ uv --version
uv 0.4.6 (Homebrew 2024-09-05)
```
though I was seeing this with `0.4.4` as well.

---

_Comment by @bluss on 2024-09-07 09:10_

I think this has something to do with sitecustomize.py - homebrew pythons are for some reason installing such a file? And it interferes with how uv is creating the overlay effect (ephemeral virtualenv layered on top of .venv).

breadcrumbs

https://github.com/Homebrew/homebrew-core/pull/133490

https://docs.brew.sh/Homebrew-and-Python

---

_Referenced in [astral-sh/uv#7161](../../astral-sh/uv/pulls/7161.md) on 2024-09-07 09:38_

---

_Closed by @charliermarsh on 2024-09-09 13:19_

---
