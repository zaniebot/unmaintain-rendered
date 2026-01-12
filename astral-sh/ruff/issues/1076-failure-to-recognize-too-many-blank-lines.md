```yaml
number: 1076
title: Failure to recognize too many blank lines
type: issue
state: closed
author: facundobatista
labels:
  - rule
assignees: []
created_at: 2022-12-05T20:52:47Z
updated_at: 2022-12-06T20:24:32Z
url: https://github.com/astral-sh/ruff/issues/1076
synced_at: 2026-01-12T15:54:41Z
```

# Failure to recognize too many blank lines

---

_@facundobatista_

Check the following code:

```
a = 100



b = 100
```

Flake8 will report `E303 too many blank lines (3)` but ruff passes silently.

```
$ fades -d flake8 -x flake8 --version
6.0.0 (mccabe: 0.7.0, pycodestyle: 2.10.0, pyflakes: 3.0.1) CPython 3.10.6 on Linux
$ fades -d flake8 -x flake8 x.py 
x.py:5:1: E303 too many blank lines (3)
$
$ fades -d ruff -x ruff --version
ruff 0.0.138
$ fades -d ruff -x ruff x.py 
$ 
```


---

_Label `rule` added by @charliermarsh on 2022-12-05 21:07_

---

_Comment by @charliermarsh on 2022-12-05 21:08_

We don't support `E303` right now, so this is expected.

---

_Comment by @facundobatista on 2022-12-05 21:29_

Will you support it in the future? Thanks!

---

_Comment by @facundobatista on 2022-12-05 21:30_

The README says 

"""
Ruff can be used as a drop-in replacement for Flake8 when used (1) without or with a small number of plugins, (2) alongside Black, and (3) on Python 3 code.

Under those conditions, Ruff implements every rule in Flake8, with the exception of F811.
"""

Are there other missing codes in comparison to flake8? Thanks!

---

_Comment by @charliermarsh on 2022-12-05 21:46_

I might! The thing with `E303` and other rules is that, IIUC, if you run Black over your code, it's effectively impossible to trigger them. Given Black's popularity, I intentionally deprioritized stylistic rules that are already fixed by Black. Eventually, I'd like to extend Ruff to do autoformatting which would effectively include automatically fixing those lint errors.


---

_Comment by @charliermarsh on 2022-12-06 20:24_

Closing in favor of #1073.

---

_Closed by @charliermarsh on 2022-12-06 20:24_

---
