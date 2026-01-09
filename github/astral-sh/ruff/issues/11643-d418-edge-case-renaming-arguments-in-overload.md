---
number: 11643
title: "D418 edge case: renaming arguments in `overload`"
type: issue
state: open
author: jamesbraza
labels:
  - docstring
assignees: []
created_at: 2024-05-31T17:44:35Z
updated_at: 2024-12-09T11:26:41Z
url: https://github.com/astral-sh/ruff/issues/11643
synced_at: 2026-01-07T13:12:15-06:00
---

# D418 edge case: renaming arguments in `overload`

---

_Issue opened by @jamesbraza on 2024-05-31 17:44_

With `ruff==0.4.6`, [D418](https://docs.astral.sh/ruff/rules/overload-with-docstring/) will get thrown when there is a docstring in a `@overload`'d signature. This makes sense in the most common case where there's some type variation but no argument renames.

However, imagine the argument names themselves change too, like in the below (inspired by the first snippet in https://peps.python.org/pep-0484/#function-method-overloading):

```python
from __future__ import annotations

from typing import overload


class Foo:
    @overload
    def __getitem__(self, i: int) -> int:
        """
        Do something with an index.

        Args:
            i: Index.
            
        Returns:
            Some integer.
        """

    @overload
    def __getitem__(self, s: slice) -> Foo:
        """
        Do something with a slice.

        Args:
            s: Slice.

        Returns:
            Some object.
        """

    def __getitem__(self, *args, **kwargs):
        """Do something."""
```

Currently, Ruff will trigger `D418` on the overloads, saying functions with `@overload` shouldn't contain a docstring.

However, I think that `D418` should not be triggered, because the argument names themselves change, and this it's impossible to construct a proper docstring in final implementation of the method.

What do you think?

---

_Label `docstring` added by @charliermarsh on 2024-05-31 17:45_

---

_Comment by @charliermarsh on 2024-05-31 17:46_

That's interesting... My first reaction is that I agree with you but interested to see what others think.

---

_Comment by @jamesbraza on 2024-05-31 18:02_

I am wondering if per-`overload` docstrings give IDEs room to render different docstrings when different arguments are specified in a hover-over menu

---

_Comment by @alicederyn on 2024-12-09 11:26_

VS Code shows per-overload docstrings. I think this check should probably be removed 🙏 

---
