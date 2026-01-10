```yaml
number: 3910
title: "Feature request: ignore `N802` for methods marked with `@override`"
type: issue
state: closed
author: Avasam
labels:
  - bug
assignees: []
created_at: 2023-04-08T00:06:08Z
updated_at: 2023-04-08T03:11:51Z
url: https://github.com/astral-sh/ruff/issues/3910
synced_at: 2026-01-10T11:09:46Z
```

# Feature request: ignore `N802` for methods marked with `@override`

---

_Issue opened by @Avasam on 2023-04-08 00:06_

Same as https://github.com/PyCQA/pep8-naming/issues/215 I think this would be useful whether it is added to pep8-naming or not

Python 3.12 will introduce the `override` decorator. It is already backported by `typing_extensions`.

Overridden method names are out of the dev's control when subclassing an external library. A strong example of this is PyQt6/PySide6, where I have to keep adding to `ignore-names` as I use more and more features. Ruff could understand that there's nothing the dev can do about the names if a method is marked with `@override` (and it'll still raise anyway on the base class if it's internal).

```
[tool.ruff.pep8-naming]
# PyQt methods
ignore-names = ["closeEvent", "paintEvent", "keyPressEvent", "mousePressEvent", "mouseMoveEvent", "mouseReleaseEvent"]
```

---

_Comment by @charliermarsh on 2023-04-08 02:51_

Makes sense!

---

_Label `bug` added by @charliermarsh on 2023-04-08 02:52_

---

_Closed by @charliermarsh on 2023-04-08 03:11_

---
