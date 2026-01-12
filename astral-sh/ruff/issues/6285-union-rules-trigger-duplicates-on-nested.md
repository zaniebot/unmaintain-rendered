```yaml
number: 6285
title: Union rules trigger duplicates on nested violations
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-08-02T19:45:44Z
updated_at: 2023-08-07T19:17:28Z
url: https://github.com/astral-sh/ruff/issues/6285
synced_at: 2026-01-12T15:54:46Z
```

# Union rules trigger duplicates on nested violations

---

_@charliermarsh_

In `expression.rs`, we have some logic to avoid checking nested `Union` operators, since our `Union` checks operate recursively (and so re-checking nested `Union` operators leads to duplicate diagnostics).

These checks don't quite work in `.pyi` files due to the way we process deferred nodes. We _don't_ store the current expression stack when snapshotting the `SemanticModel`, because that would require us to store all expressions in an `IndexVec`. This is a known source of bugs.

We can store the expressions in an `IndexVec`, like we do for statements, but it will definitely hurt performance and increase memory usage.

As an example, if this is put in a `.pyi` file:

```python
from typing import Union

Union[int, Union[int, int]]
```

You'll hit duplicate violations:

```
foo.pyi:3:22: PYI016 Duplicate union member `int`
foo.pyi:3:27: PYI016 Duplicate union member `int`
foo.pyi:3:27: PYI016 Duplicate union member `int`
```


---

_Label `bug` added by @charliermarsh on 2023-08-02 19:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-07 17:36_

---

_Closed by @charliermarsh on 2023-08-07 19:17_

---
