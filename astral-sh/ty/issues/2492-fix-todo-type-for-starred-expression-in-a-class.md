```yaml
number: 2492
title: "Fix `@Todo` type for starred expression in a `class` statement"
type: issue
state: open
author: AlexWaygood
labels: []
assignees: []
created_at: 2026-01-14T14:26:30Z
updated_at: 2026-01-15T04:06:31Z
url: https://github.com/astral-sh/ty/issues/2492
synced_at: 2026-01-15T04:51:36Z
```

# Fix `@Todo` type for starred expression in a `class` statement

---

_@AlexWaygood_

For example:

```py
from ty_extensions import reveal_mro
bases = (int, str)

class Foo(*bases): ...

reveal_mro(Foo)  # revealed: (<class 'Foo'>, @Todo(StarredExpression), <class 'object'>)
```

https://play.ty.dev/5fb011e6-cbd6-48b8-b4e7-95c98d6ca76b

I wouldn't say that this is _very_ high priority, because it just doesn't come up that much, but we should resolve it at Some Point™️

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-14 14:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-15 04:06_

---
