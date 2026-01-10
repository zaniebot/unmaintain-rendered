```yaml
number: 2756
title: Ensure that line-length and doc-length behavior is consistent with Flake8
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-02-11T04:06:29Z
updated_at: 2023-05-16T13:45:53Z
url: https://github.com/astral-sh/ruff/issues/2756
synced_at: 2026-01-10T11:09:45Z
```

# Ensure that line-length and doc-length behavior is consistent with Flake8

---

_Issue opened by @charliermarsh on 2023-02-11 04:06_

See: https://github.com/charliermarsh/ruff/issues/1784#issuecomment-1426597334

---

_Label `bug` added by @charliermarsh on 2023-02-11 04:34_

---

_Comment by @charliermarsh on 2023-04-01 16:07_

On further investigation, it seems that our current behavior _is_ consistent with Flake8.

---

_Closed by @charliermarsh on 2023-04-01 16:07_

---

_Comment by @LukasMinogue on 2023-05-16 11:23_

@charliermarsh 

> On further investigation, it seems that our current behavior _is_ consistent with Flake8.

https://github.com/charliermarsh/ruff/blob/6049aabe27ee536d40c8cf6f446f475d1a937318/ruff.schema.json#L1339-L1340

As far as I can see, that is not the case. It does not work for docstrings, nor single comments (isolated or at the end of a line).

I have several ``E501 Line too long`` warnings when using comments, that flake would ignore. I would not even mind, but since there is no auto-fix for this, it makes this a unwelcomed hassle and I could not find a way to change it or turn it off for just the comments.

I am using ruff 0.0.267.

**My ``pyproject.toml`` file**
```
...
[tool.ruff]
line-length = 88
...

[tool.ruff.pycodestyle]
max-doc-length = 180
...
```


**Example file** 
```py
import numpy as np # this is a long comment increasing the length of this line over 88 characters

# this is a very very very very very very very very very long single line comment with a length over 88 characters.

def func():
    """this is a very very very very very very very very very long doc string with a length over 88 characters."""
    print(np.array([1,2,3]))
```

**Warnings**
```bash
Untitled-1.py:2:89: E501 Line too long (97 > 88 characters)
Untitled-1.py:4:89: E501 Line too long (115 > 88 characters)
Untitled-1.py:7:89: E501 Line too long (114 > 88 characters)
Found 3 errors.
```

---

_Comment by @charliermarsh on 2023-05-16 13:00_

Do you mind sharing your Flake8 configuration? In my experience and testing, Flake8 flags all of those line as E501:

```shell
> flake8 foo.py --select E501 --isolated
foo.py:1:80: E501 line too long (97 > 79 characters)
foo.py:3:80: E501 line too long (115 > 79 characters)
foo.py:6:80: E501 line too long (114 > 79 characters)
```

---

_Comment by @charliermarsh on 2023-05-16 13:07_

Also note that Ruff doesn't enable the `W` rules by default:

```
❯ ruff foo --select W505
foo/foo.py:3:101: W505 Doc line too long (115 > 100 characters)
foo/foo.py:6:101: W505 Doc line too long (114 > 100 characters)
```

And that Flake8 also doesn't flag end-of-line comments as "doc lines":

```shell
❯ flake8 foo.py --select W505 --max-doc-length 100 --isolated
foo.py:3:101: W505 doc line too long (115 > 100 characters)
foo.py:6:101: W505 doc line too long (114 > 100 characters)
```

---

_Comment by @LukasMinogue on 2023-05-16 13:45_

> Do you mind sharing your Flake8 configuration? In my experience and testing, Flake8 flags all of those line as E501:

Excuse me, I overlooked the Error in my ignore list. You are right.

---
