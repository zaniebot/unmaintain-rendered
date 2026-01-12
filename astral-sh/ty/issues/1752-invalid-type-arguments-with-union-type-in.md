```yaml
number: 1752
title: "`invalid-type-arguments` with `Union` type in `TypeAlias`"
type: issue
state: closed
author: cmp0xff
labels: []
assignees: []
created_at: 2025-12-04T12:30:57Z
updated_at: 2025-12-04T13:38:28Z
url: https://github.com/astral-sh/ty/issues/1752
synced_at: 2026-01-12T15:54:25Z
```

# `invalid-type-arguments` with `Union` type in `TypeAlias`

---

_@cmp0xff_

### Summary

Hi, the following code snippet causes `invalid-type-arguments` in current `ty`, see [the playground](https://play.ty.dev/4db31eef-aad0-4fcd-b85a-5a7d24c488c9):

```py
from collections.abc import Callable, Mapping
from typing import ParamSpec, TypeAlias

P = ParamSpec("P")

Fun: TypeAlias = Callable[P, object] | str
FunDict: TypeAlias = Mapping[object, Fun[P]]  # Too many type arguments: expected 0, got 1 (invalid-type-arguments) [Ln 7, Col 42]
```

It passes the check of `mypy` and `pyright`.

The issue arose from pandas-dev/pandas-stubs#1526.

### Version

b8ecc83a5

---

_Comment by @dhruvmanila on 2025-12-04 13:38_

Thank you for the bug report!

This will be fixed by https://github.com/astral-sh/ruff/pull/21445 which adds support for `ParamSpec` (I tried your example locally).

---

_Closed by @dhruvmanila on 2025-12-04 13:38_

---
