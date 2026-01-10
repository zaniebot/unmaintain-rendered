```yaml
number: 940
title: "Allow excluding functions with some decorators (like `@override`) from the docstring rules"
type: issue
state: closed
author: tmke8
labels: []
assignees: []
created_at: 2022-11-28T12:37:45Z
updated_at: 2022-11-28T20:42:56Z
url: https://github.com/astral-sh/ruff/issues/940
synced_at: 2026-01-10T12:09:58Z
```

# Allow excluding functions with some decorators (like `@override`) from the docstring rules

---

_Issue opened by @tmke8 on 2022-11-28 12:37_

I use [typing_extensions.override](https://github.com/python/typing_extensions/blob/main/CHANGELOG.md#release-440-october-6-2022) to annotate methods that are overriding a method in the super class. The overriding methods don't need (but of course can optionally have) docstrings because the super method already has one.

---

_Comment by @charliermarsh on 2022-11-28 14:40_

I thought this was supported, let me take a look...

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-28 14:40_

---

_Comment by @charliermarsh on 2022-11-28 14:42_

Oh, I was thinking of `@overload`, not `@override`!

---

_Closed by @charliermarsh on 2022-11-28 14:52_

---

_Comment by @charliermarsh on 2022-11-28 20:42_

This is going out in [v0.0.143](https://github.com/charliermarsh/ruff/releases/tag/v0.0.143).

---
