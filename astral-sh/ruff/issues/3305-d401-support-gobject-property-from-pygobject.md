---
number: 3305
title: "D401: Support GObject.Property from PyGObject"
type: issue
state: closed
author: staticssleever668
labels:
  - question
  - docstring
assignees: []
created_at: 2023-03-02T13:25:32Z
updated_at: 2023-03-02T18:53:24Z
url: https://github.com/astral-sh/ruff/issues/3305
synced_at: 2026-01-10T01:22:41Z
---

# D401: Support GObject.Property from PyGObject

---

_Issue opened by @staticssleever668 on 2023-03-02 13:25_

* Ruff version: 3ed539d50ed6260358c97d2e00b49bad4abfa37e (0.0.253)

```py
from gi.repository import GObject


class Thingy(GObject.GObject):
    _beep = "boop"

    @GObject.Property
    def good_property(self):
        """This property method docstring does not need to be written in imperative mood."""
        return self._beep

my_thingy = Thingy()
print(my_thingy.good_property)
```

When run this as expected prints `boop`.
But `ruff check --isolated --select D401 test.py` gives a D401 warning: 

```
test.py:9:9: D401 First line of docstring should be in imperative mood: "This property method docstring does not need to be written in imperative mood."
```

I made a branch that removes this warning for GObject.Property (https://github.com/staticssleever668/ruff/tree/feat_support_gobject_property), but I'm not sure if support for third-party libraries in such cases makes sense.

As a note, pydocstyle produces a warning for this as well:
```
test.py:9 in public method `good_property`:
        D401: First line should be in imperative mood; try rephrasing (found 'This')
```

---

_Comment by @charliermarsh on 2023-03-02 15:31_

I think this would be solved by the `property_decorators` setting here: https://github.com/charliermarsh/ruff/issues/2205. Does that sound right to you? I.e., to list `GObject` as a property decorator.

---

_Label `question` added by @charliermarsh on 2023-03-02 15:31_

---

_Label `docstring` added by @charliermarsh on 2023-03-02 15:31_

---

_Comment by @staticssleever668 on 2023-03-02 18:52_

Yes, it's good! :+1:

```toml
[tool.ruff.pydocstyle]
ignore-decorators=["gi.repository.GObject.Property"]
```

---

_Closed by @staticssleever668 on 2023-03-02 18:52_

---

_Comment by @charliermarsh on 2023-03-02 18:53_

I think the unimplemented `property_decorators` would be even better, since then we'd still lint the docstring, we just wouldn't enforce imperative style.

---
