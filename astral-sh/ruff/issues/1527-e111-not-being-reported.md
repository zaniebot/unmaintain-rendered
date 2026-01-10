```yaml
number: 1527
title: E111 not being reported?
type: issue
state: closed
author: MarcoGorelli
labels: []
assignees: []
created_at: 2023-01-01T09:42:09Z
updated_at: 2023-01-01T09:55:05Z
url: https://github.com/astral-sh/ruff/issues/1527
synced_at: 2026-01-10T12:05:30Z
```

# E111 not being reported?

---

_Issue opened by @MarcoGorelli on 2023-01-01 09:42_

Hi,

I may be misunderstanding something, but from the README

> By default, Ruff enables all E and F error codes, which correspond to those built-in to Flake8.

I thought that all E* codes from flake8 would show up with `ruff` too. But here I see `E111` being skipped:

Example:
```console
(.venv) marcogorelli@DESKTOP-U8OKFP3:~/tmp$ cat t.py
if 2 + 5:
  pass
(.venv) marcogorelli@DESKTOP-U8OKFP3:~/tmp$ flake8 t.py
t.py:2:3: E111 indentation is not a multiple of 4
(.venv) marcogorelli@DESKTOP-U8OKFP3:~/tmp$ ruff t.py
```

Diagnostics:
```console
(.venv) marcogorelli@DESKTOP-U8OKFP3:~/tmp$ ruff --version
ruff 0.0.205
(.venv) marcogorelli@DESKTOP-U8OKFP3:~/tmp$ flake8 --version
6.0.0 (mccabe: 0.7.0, pycodestyle: 2.10.0, pyflakes: 3.0.1) CPython 3.8.16 on Linux
(.venv) marcogorelli@DESKTOP-U8OKFP3:~/tmp$ python --version
Python 3.8.16
```

Likewise for `E226`

---

_Comment by @squiddy on 2023-01-01 09:52_

Hey,

that refers to all error codes as currently implemented by ruff. `E111` is not one of them. One is able to replace flake8 with ruff under conditions mentioned [here](https://github.com/charliermarsh/ruff#how-does-ruff-compare-to-flake8).

> Ruff can be used as a drop-in replacement for Flake8 when used (1) without or with a small number of plugins, (2) alongside Black, and (3) on Python 3 code.

None of error codes < E400 from pycodestyle have been implemented so far. A more recent comment from Charlie on that matter:

> We prioritized the rules based on the assumption that Ruff would be used alongside Black -- so we just haven't focused on implementing rules like `E302` that are made obsolete by an autoformatter. (It doesn't mean we won't implement them, just that they weren't / haven't been prioritized.) That's one of the key assumptions baked into any "parity with Flake8" claims. I hope it doesn't come across as misleading, it's all documented in the [FAQ](https://github.com/charliermarsh/ruff#how-does-ruff-compare-to-flake8), definitely not my intent.

_Originally posted by @charliermarsh in https://github.com/charliermarsh/ruff/issues/1073#issuecomment-1353347782_
      
That said, eventually becoming an autoformatter is in scope of this project.

---

_Comment by @MarcoGorelli on 2023-01-01 09:55_

hey, that's clear, thanks!

---

_Closed by @MarcoGorelli on 2023-01-01 09:55_

---
