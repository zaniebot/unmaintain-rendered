---
number: 9004
title: Disable ANN201 None-autofix for abstractmethods?
type: issue
state: closed
author: twoertwein
labels:
  - rule
assignees: []
created_at: 2023-12-05T04:10:43Z
updated_at: 2023-12-07T02:47:38Z
url: https://github.com/astral-sh/ruff/issues/9004
synced_at: 2026-01-07T13:12:15-06:00
---

# Disable ANN201 None-autofix for abstractmethods?

---

_Issue opened by @twoertwein on 2023-12-05 04:10_

Abstract methods often have an almost empty body (doc-string, pass, or raise) but it seems unlikely that they should return `None`. https://github.com/JelleZijlstra/autotyping does not add `None` for `abstractmethod`s.
```py
import abc

class Test(abc.ABC):
    @abc.abstractmethod
    def test(self):  #ruff adds  -> None
        """sub-classes have to implement this"""
```
```sh
$ ruff --version
ruff 0.1.7
$ ruff --select ANN2 --exit-non-zero-on-fix --unsafe-fixes test.py 
Found 1 error (1 fixed, 0 remaining).
```

---

_Label `rule` added by @charliermarsh on 2023-12-05 04:12_

---

_Comment by @charliermarsh on 2023-12-05 04:12_

Seems reasonable to me.

---

_Comment by @twoertwein on 2023-12-05 04:15_

(and also not add None return types to pyi files)

edit: other return types can make sense (`def __str__` should also return str in a pyi files).

---

_Comment by @charliermarsh on 2023-12-05 20:09_

I guess I'm not sure if we should flag these at all for abstract methods with empty bodies. Probably not.

---

_Comment by @charliermarsh on 2023-12-05 20:10_

Sorry, actually, we should flag.

---

_Comment by @twoertwein on 2023-12-05 22:13_

> Sorry, actually, we should flag.

ANN201 should definitely report missing return annotations for abstract methods and in pyi files, but autofixing those two error cases based on the "non-existing" function body with `-> None` seems not helpful to me. As in the case of autotyping https://github.com/JelleZijlstra/autotyping/issues/33  it is still helpful to add `-> None` based on the name of the method, such as for `__init__` (even when it is abstract or in a pyi file).

I stumbled into these cases when trying to run ruff with the preview fixes on pandas.

---

_Referenced in [astral-sh/ruff#9034](../../astral-sh/ruff/pulls/9034.md) on 2023-12-07 02:35_

---

_Closed by @dhruvmanila on 2023-12-07 02:47_

---

_Referenced in [astral-sh/ruff#9270](../../astral-sh/ruff/issues/9270.md) on 2023-12-25 03:17_

---
