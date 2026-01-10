```yaml
number: 138
title: "`F821 Undefined name` false negatives"
type: issue
state: closed
author: nikolaik
labels:
  - bug
assignees: []
created_at: 2022-09-10T11:49:40Z
updated_at: 2022-09-10T19:19:18Z
url: https://github.com/astral-sh/ruff/issues/138
synced_at: 2026-01-10T15:56:05Z
```

# `F821 Undefined name` false negatives

---

_Issue opened by @nikolaik on 2022-09-10 11:49_

Love your work on Ruff, congrats!  ❤️

Really want to start using it soon! When initially evaluating I found a few cases where rule `F821` was not happy with me:

**Using star imports**

a.py
```python
YO = 'lo' 
```

b.py
```python
from .a import *

if YO == 'lo':  #  F821 Undefined name `YO`
    YO = 'swag'
```

**Using unpacked list values**

```python
[first] = ['yup'] #  F821 Undefined name `first`
```
   
**Inline functional declaration of typed dicts**

```python
from typing import TypedDict

class Item(TypedDict):
    nodes = list[TypedDict('Node', {'id': str})]  #  F821 Undefined name `Node`
```

---

_Comment by @charliermarsh on 2022-09-10 14:10_

Thank you! These are great. The first and third issue were known to me but not documented anywhere (and the second is definitely a bug). Will fix.

---

_Label `bug` added by @charliermarsh on 2022-09-10 14:11_

---

_Comment by @charliermarsh on 2022-09-10 15:15_

On star imports: right now, the best we could do for now (which is equivalent to Flake8 / PyFlakes IIUC) is provide a separate error code and message, like: `%r may be undefined, or defined from star imports: %s`. (Doing anything more advanced would break the single-file-at-a-time design of the linter, since we'd have to look at the import tree to understand what was brought in from the star import.)

---

_Comment by @charliermarsh on 2022-09-10 16:53_

`Using unpacked list values` has been fixed in d7f95ac6b6e7775306c465a7b59e57da4e7c62d9.

---

_Comment by @charliermarsh on 2022-09-10 17:05_

`Inline functional declaration of typed dicts` has been fixed in `6a24351202c009c14997dbb545cfb50e5393c210`.

---

_Comment by @charliermarsh on 2022-09-10 17:06_

v0.0.31 is building now with those fixes. Going to close and create a separate issue to track starred imports. Thank you!

---

_Closed by @charliermarsh on 2022-09-10 17:06_

---

_Comment by @nikolaik on 2022-09-10 19:04_

Thanks for all the fixes, much improved!

Testing with v0.0.31 with the fix for _Inline functional declaration of typed dicts_ in 6a24351202c009c14997dbb545cfb50e5393c210 I still see errors with:

```python
# yolo.py
from typing import TypedDict

class Item(TypedDict):
    nodes: list[TypedDict('Node', {'name': str})]
```

```console
$ ruff yolo.py
yolo.py:5:37: F821 Undefined name `name`

Found 1 error(s).
```

The reason why the test is passing [this line](https://github.com/charliermarsh/ruff/commit/6a24351202c009c14997dbb545cfb50e5393c210#diff-354badb80ef8370f6ea60e37eaf0074684656616254f547bedc1253fb3c81818R67), I'm guessing, is that `id` happens to be a keyword in python 


---

_Comment by @charliermarsh on 2022-09-10 19:19_

Gah, good call -- should be fixed in `c247730bf5d6394cfd4715c35ebb7d08e0da4fd0`.

---
