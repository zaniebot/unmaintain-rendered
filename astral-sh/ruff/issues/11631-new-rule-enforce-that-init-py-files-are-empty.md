```yaml
number: 11631
title: "new rule - enforce that `__init__.py` files are empty"
type: issue
state: closed
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-05-31T06:57:45Z
updated_at: 2024-06-01T23:34:52Z
url: https://github.com/astral-sh/ruff/issues/11631
synced_at: 2026-01-12T15:54:51Z
```

# new rule - enforce that `__init__.py` files are empty

---

_@DetachHead_

putting any code in `__init__.py` files is a bad idea, because they are unnecessarily imported when you import any modules in the same folder:

```py
# ./package1/__init__.py
from package2.baz import baz
```
```py
# ./package1/bar.py
bar = 1
```
```py
# ./package2/baz.py
from package1.bar import bar
baz = 2
```
```py
# ./package2/main.py
from package2.baz import baz
```

in this example, running `python package2/main.py` crashes with a circular import error, because `package1.__init__` is imported even though none of the other files are explicitly importing from it.

it's absurd that the language would implicitly import a file that was not requested at all, so it would be nice if there was a rule to enforce that `__init__.py` files are empty.

---

_Label `rule` added by @charliermarsh on 2024-05-31 23:20_

---

_Label `needs-decision` added by @charliermarsh on 2024-05-31 23:20_

---

_Comment by @charliermarsh on 2024-06-01 23:34_

I believe this can be merged with https://github.com/astral-sh/ruff/issues/9848.

---

_Closed by @charliermarsh on 2024-06-01 23:34_

---
