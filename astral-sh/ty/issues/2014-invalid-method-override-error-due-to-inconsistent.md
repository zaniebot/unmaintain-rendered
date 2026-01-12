```yaml
number: 2014
title: "`invalid-method-override` error due to inconsistent third-party stubs"
type: issue
state: closed
author: LoicRiegel
labels:
  - question
assignees: []
created_at: 2025-12-17T14:08:27Z
updated_at: 2025-12-17T14:40:29Z
url: https://github.com/astral-sh/ty/issues/2014
synced_at: 2026-01-12T15:54:26Z
```

# `invalid-method-override` error due to inconsistent third-party stubs

---

_@LoicRiegel_

### Question

### Problem

I ran into this situation where I get an `invalid-method-override` error when working with PySide6 classes, caused by inconsistencies in third-party stubs.

In short: I cannot write an override that is accepted against all base classes in the MRO, even though the override is runtime-correct and type-correct.

Minimal example:
```py
from typing import override
from PySide6.QtGui import QKeyEvent
from PySide6.QtWidgets import QTreeView

class MyTreeView(QTreeView):
    @override
    def keyPressEvent(self, event: QKeyEvent) -> None:
        super().keyPressEvent(event)
```
and run ``ty check`` on it with ``PySide6`` and ``PySide6-stubs`` in the environment.

```
> ty check tmp\tmp.py
error[invalid-method-override]: Invalid override of method `keyPressEvent`
   --> tmp\tmp.py:9:9
    |
  7 | class MyTreeView(QTreeView):
  8 |     @override
  9 |     def keyPressEvent(self, event: QKeyEvent) -> None:
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Definition is incompatible with `QAbstractScrollArea.keyPressEvent`
 10 |         return None
    |
   ::: .venv\Lib\site-packages\PySide6-stubs\QtWidgets.pyi:367:9
    |
365 |     def horizontalScrollBar(self) -> PySide6.QtWidgets.QScrollBar: ...
366 |     def horizontalScrollBarPolicy(self) -> PySide6.QtCore.Qt.ScrollBarPolicy: ...
367 |     def keyPressEvent(self, arg__1: PySide6.QtGui.QKeyEvent) -> None: ...
    |         ------------------------------------------------------------ `QAbstractScrollArea.keyPressEvent` defined here
368 |     def maximumViewportSize(self) -> PySide6.QtCore.QSize: ...
369 |     def minimumSizeHint(self) -> PySide6.QtCore.QSize: ...
    |
info: This violates the Liskov Substitution Principle
info: rule `invalid-method-override` is enabled by default

Found 1 diagnostic
```


The reason is that PySide6 stubs define ``keyPressEvent`` with different parameter names in different base classes (signature is the same otherwise):

```py
QTreeView.keyPressEvent(self, event: QKeyEvent) -> None: ...
QAbstractScrollArea.keyPressEvent(self, arg__1: QKeyEvent) -> None: ...
```

So I'm stuck, because ``ty`` will check ``MyTreeView`` against all parents in the MRO chain, I'm either compatible with ``QTreeView`` and incompatible with ``QAbstractScrollArea``, or incompatible with ``QTreeView`` and compatible with ``QAbstractScrollArea``.

Note that this happens regardless of whether ``@typing.override`` is present.

### Comparison with mypy

Mypy raises no error. I think this is due to the fact that it intentionally ignores parameter names for overrides.
https://mypy.readthedocs.io/en/stable/error_code_list.html?utm_source=chatgpt.com#check-validity-of-overrides-override

### Question

Sure, one solution would be to fix the stubs upstream so they don't break LSP in the first place, but it would be nice if we could do something without this.

I could add ``# ty: ignore[invalid-method-override]`` but then I'd give up all the checks.

Is there a recommended approach here?

For example:
- Would relaxing name-only checks (or making them optional) be acceptable in cases like this?
- Or is the expectation that users suppress the diagnostic when working with inconsistent third-party stubs?

### Version

ty 0.0.1-alpha.35 (3188a56be 2025-12-16)

---

_Label `question` added by @LoicRiegel on 2025-12-17 14:08_

---

_Comment by @ntBre on 2025-12-17 14:20_

Thanks for the report! I'll send this over to the ty repository :)

(The code and PRs for ty live here, but issues are over there)

---

_Comment by @AlexWaygood on 2025-12-17 14:27_

Thanks very much for the report! (And thanks @ntBre for transferring over!)

I think this is a similar issue to https://github.com/astral-sh/ty/issues/2000?

---

_Comment by @LoicRiegel on 2025-12-17 14:38_

Oups so sorry for creating the issue in ruff, I thought I was in ty

Hmm yes, could be duplicate. the TODOs in your comment are what we need to do to solve my problem too:
> 1. We need to improve the error message to make clear that it's the parameter names that are the problem!
> 2. Ideally I think we would split errors about bad parameter names into their own, more specific error code



---

_Closed by @MichaReiser on 2025-12-17 14:40_

---
