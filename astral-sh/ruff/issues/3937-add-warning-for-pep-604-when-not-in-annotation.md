```yaml
number: 3937
title: Add warning for PEP 604 when not in annotation
type: issue
state: closed
author: wookie184
labels:
  - rule
assignees: []
created_at: 2023-04-11T17:28:32Z
updated_at: 2023-05-02T06:49:22Z
url: https://github.com/astral-sh/ruff/issues/3937
synced_at: 2026-01-12T15:54:44Z
```

# Add warning for PEP 604 when not in annotation

---

_@wookie184_

Ruff doesn't warn on this:
```python
import typing

y = typing.Union[int, str]
z = typing.Optional[str]
```

This seems to be as a result of https://github.com/charliermarsh/ruff/issues/3215, although I don't really understand the reasoning there, the same change is made if a type is used in an annotation, and as it can still be accessed at runtime it could still cause errors.

For example,
```python
from typing import Union

new_types = (int, float)
def thing(arg: Union[new_types]):
    ...
```
becomes

```python
new_types = (int, float)
def thing(arg: new_types):
    ...
```
(really the issue there seems to be invalid use of `Union` in the first place, afaik the only variables you can use there are type aliases, which that tuple is not)

This is also the case in the other issue linked, https://github.com/charliermarsh/ruff/issues/3215, where as far as I can tell the issue happens whether the union is defined in a variable or directly in the annotation.

Anyway, I don't really mind too much about autofixes, but I think it would be nice to be able to have a warning for it regardless.


---

_Label `rule` added by @charliermarsh on 2023-04-12 04:04_

---

_Comment by @charliermarsh on 2023-04-12 04:09_

I think it makes sense to warn on them at least. I'm hesitant to fix since it can introduce subtle bugs, such as that identified in #3215, whereby replacing `Union[new_types]` with `new_types` leads to a runtime error (since we can't know that `new_types` is a tuple there, and not a single type).

---

_Closed by @charliermarsh on 2023-05-02 06:49_

---
