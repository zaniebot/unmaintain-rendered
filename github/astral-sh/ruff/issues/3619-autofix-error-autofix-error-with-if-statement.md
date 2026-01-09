---
number: 3619
title: "[Autofix error] Autofix error with if statement"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-20T08:12:24Z
updated_at: 2023-06-12T00:28:19Z
url: https://github.com/astral-sh/ruff/issues/3619
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix error] Autofix error with if statement

---

_Issue opened by @qarmin on 2023-03-20 08:12_

Ruff  a45753f462c7a53afd7f942ab3c6f9af3871bf1f

```
def _parseline(path: str, line: str, lineno: int) -> tuple[str | None, str | None]:
    if iscommentline(line):
        line = ""
    else:
        line = *line.rstrip()
```
[25633_parse9.py.zip](https://github.com/charliermarsh/ruff/files/11025388/25633_parse9.py.zip)

with 
```
ruff file.py --fix
```

```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Desktop/RunEveryCommand/Ruff/Broken/17333_parse90.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```


---

_Label `bug` added by @charliermarsh on 2023-03-20 15:11_

---

_Comment by @charliermarsh on 2023-03-20 21:18_

Is the initial example valid Python? I can't get that to parse under Python 3.11.

---

_Comment by @charliermarsh on 2023-03-20 21:18_

```
❯ python foo.py
  File "/Users/crmarsh/workspace/ruff/foo.py", line 5
    line = *line.rstrip()
           ^^^^^^^^^^^^^^
SyntaxError: can't use starred expression here
```

---

_Label `question` added by @charliermarsh on 2023-03-20 23:23_

---

_Comment by @qarmin on 2023-03-21 07:31_

I cannot find original file with this lines.

I tried to run this with several python versions and with each I had parse error, so the problem here is probably with recognizing this file by ruff as valid 

---

_Comment by @charliermarsh on 2023-03-21 14:26_

I think it's an error that Ruff doesn't flag the _original_ source as invalid syntax.

---

_Label `question` removed by @charliermarsh on 2023-03-21 14:26_

---

_Comment by @qarmin on 2023-05-28 18:41_

Such files I reported upstream - https://github.com/RustPython/Parser/issues/66

---

_Comment by @charliermarsh on 2023-06-12 00:28_

I'm going to close this one in favor of the RustPython issue.

---

_Closed by @charliermarsh on 2023-06-12 00:28_

---
