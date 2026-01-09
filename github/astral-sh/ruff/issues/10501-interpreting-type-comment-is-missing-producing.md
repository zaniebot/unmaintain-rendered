---
number: 10501
title: Interpreting type comment is missing, producing different result from flake8
type: issue
state: closed
author: cielavenir
labels:
  - question
assignees: []
created_at: 2024-03-21T03:08:23Z
updated_at: 2024-03-22T00:09:25Z
url: https://github.com/astral-sh/ruff/issues/10501
synced_at: 2026-01-07T13:12:15-06:00
---

# Interpreting type comment is missing, producing different result from flake8

---

_Issue opened by @cielavenir on 2024-03-21 03:08_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Version

```
$ ruff --version
ruff 0.3.3
$ python3 -m flake8 --version
3.9.2 (flake8_logger: 0.0.0, mccabe: 0.6.1, pycodestyle: 2.7.0, pyflakes: 2.3.1, teamcity-messages: 1.21) CPython 3.9.2 on Linux
```

Code with import:

```py:typecomment1.py
import collections


class ToyClass(object):
    dic = None  # type: collections.defaultdict
```

Result:

```
$ python3 -m flake8 typecomment1.py 
$ ruff check typecomment1.py 
typecomment1.py:1:8: F401 [*] `collections` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Code without import:

```py:typecomment2.py
class ToyClass(object):
    dic = None  # type: collections.defaultdict
```

Result:

```
$ python3 -m flake8 typecomment2.py
typecomment.py:2:17: F821 undefined name 'collections'
$ ruff check typecomment2.py 
All checks passed!
```

This means that Ruff does not interpret type comment while flake8 does.

I searched for "type comment".

----

Found by a collegue of mine @braineo .

---

_Comment by @AlexWaygood on 2024-03-21 08:10_

Thanks for the report!

The latest version of flake8 also does not understand type comments; flake8 3.9.2 is several years old now. It dropped support so that it could improve performance, and because there's no reason to use type comments unless you support very old Python versions that have been end-of-life for a long time. The relevant change here is in one of the builtin flake8 plugins, pyflakes:
- https://github.com/PyCQA/pyflakes/pull/684

Since great tools such as https://github.com/ilevkivskyi/com2ann exist that can help people auto-upgrade their legacy type comments to type annotations, I'm therefore sceptical that ruff should add support for type comments.

---

_Label `question` added by @MichaReiser on 2024-03-21 08:16_

---

_Comment by @AlexWaygood on 2024-03-21 09:46_

It looks like this issue is actually a duplicate of #1619, so I'll close in favour of that one 👍

---

_Closed by @AlexWaygood on 2024-03-21 09:46_

---

_Comment by @felixvd on 2024-03-22 00:09_

That's a fair point. Thanks for your prompt and detailed response @AlexWaygood 

---

_Referenced in [astral-sh/ruff#13925](../../astral-sh/ruff/issues/13925.md) on 2024-10-25 14:21_

---
