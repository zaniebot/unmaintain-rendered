```yaml
number: 1113
title: "Narrowing down with `isinstance` not working properly"
type: issue
state: closed
author: mflova
labels:
  - narrowing
assignees: []
created_at: 2025-09-01T10:07:11Z
updated_at: 2025-09-02T17:53:17Z
url: https://github.com/astral-sh/ty/issues/1113
synced_at: 2026-01-10T02:06:24Z
```

# Narrowing down with `isinstance` not working properly

---

_Issue opened by @mflova on 2025-09-01 10:07_

### Summary

I found this specific case where the narrowing down is not working as expected

```py
from typing_extensions import reveal_type
from collections.abc import Sequence


def func_nok(value: Sequence[int] | None) -> None:
    if isinstance(value, list | tuple):
        reveal_type(value)  #  Sequence[int] | None


def func_ok(value: Sequence[int]) -> None:
    if isinstance(value, list | tuple):
        reveal_type(value)  #  Sequence[int]
```

Somehow `| None` is still present despite I do `isinstance(value, list | tuple)`.

### Version

0.0.1-alpha.19

---

_Comment by @sharkdp on 2025-09-01 10:21_

Thank you for reporting this. I think there are two things that need fixing here.

1. We do not support `isinstance` checks where the `classinfo` argument is a union type. See [this simpler example](https://play.ty.dev/4ca96f2e-7469-4453-a803-0c7eafd9ec05). This is tracked in https://github.com/astral-sh/ty/issues/122.
2. If you work around this, and replace the single `list | tuple` check with `isinstance(value, list) or isinstance(value, tuple)`, you'll notice that `None` is eliminated as a possibility. But then we'll infer `(Sequence[int] & list[Unknown]) | (Sequence[int] & tuple[Unknown, ...])` for `x`. See https://github.com/astral-sh/ty/issues/456 for a related discussion.

---

_Label `narrowing` added by @sharkdp on 2025-09-01 10:21_

---

_Comment by @carljm on 2025-09-02 17:53_

Thanks for the report! Going to close this as a duplicate since each aspect of it is tracked elsewhere. I think the more salient immediate issue here is the narrowing on PEP 604 union types, so I'll mark it duplicate of that one.

---

_Closed by @carljm on 2025-09-02 17:53_

---
