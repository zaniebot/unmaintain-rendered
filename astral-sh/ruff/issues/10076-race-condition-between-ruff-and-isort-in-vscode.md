```yaml
number: 10076
title: Race condition between ruff and isort in VScode
type: issue
state: closed
author: jazzlw
labels:
  - bug
assignees: []
created_at: 2024-02-21T22:54:16Z
updated_at: 2024-02-22T02:13:34Z
url: https://github.com/astral-sh/ruff/issues/10076
synced_at: 2026-01-12T15:54:49Z
```

# Race condition between ruff and isort in VScode

---

_@jazzlw_

Not sure if this is the right place for this, sorry if not. 

If you have both ruff and isort enabled in vscode, with format_on_save enabled, and then you have blank lines after the imports, it will delete lines of code after the blank lines when it deletes them. The number of code lines deleted depends on the number of blank lines.

Not a big deal, since it goes away once you realize and disable isort, but maybe would be nice to have some kind of heads up or warning or something.


To recreate:

Enable ruff and isort and format on save in VScode.

when you save the following python file:


```python
import sys







def test():
    a = 'test'
    return a


def foo(bar):
    return bar + 1
```


It becomes the following when formatted by the two (first function deleted):


```python
import sys


def foo(bar):
    return bar + 1
```

---

_Comment by @charliermarsh on 2024-02-21 23:02_

Ahh yeah, this one is unfortunate. It's tracked in the VS Code extension repo (https://github.com/astral-sh/ruff-vscode/issues/128), but it's actually a bug in VS Code itself that remains unfixed: https://github.com/microsoft/vscode/issues/174295.

Thanks for filing and sorry for the trouble.

---

_Closed by @charliermarsh on 2024-02-21 23:02_

---

_Comment by @charliermarsh on 2024-02-21 23:02_

(Just merging into the VS Code repo issue.)

---

_Label `bug` added by @charliermarsh on 2024-02-21 23:02_

---

_Comment by @jazzlw on 2024-02-22 02:13_

thanks for the quick response! 

---
