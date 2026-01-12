```yaml
number: 15940
title: "[`flake8-pyi`] Add autofix for `unused-private-type-var` (`PYI018`)"
type: issue
state: closed
author: ntBre
labels:
  - good first issue
  - fixes
  - help wanted
assignees: []
created_at: 2025-02-04T15:08:56Z
updated_at: 2025-02-06T18:08:38Z
url: https://github.com/astral-sh/ruff/issues/15940
synced_at: 2026-01-12T15:54:55Z
```

# [`flake8-pyi`] Add autofix for `unused-private-type-var` (`PYI018`)

---

_@ntBre_

We currently offer a diagnostic for unused private type variables like these:


```python
import typing
import typing_extensions

_T = typing.TypeVar("_T")
_Ts = typing_extensions.TypeVarTuple("_Ts")
```

But it would be nice to offer a fix to delete them too. This came up in #15682 because a PYI018 autofix would finish the transformation of pre-PEP-695 code like this:

```python
from typing import Generic, TypeVar

_T = TypeVar("_T")

class Foo(Generic[_T]):
    var: _T
```

to 


```python
class Foo[T]:
    var: T
```

by converting from `Generic` + standalone `TypeVar` to PEP-695 generics ([UP046](https://docs.astral.sh/ruff/rules/non-pep695-generic-class/), renaming the private generic ([UP051](https://github.com/astral-sh/ruff/pull/15862)), and then removing the now-unused private type variable (PYI018 after this issue).

The function implementing this rule can be found [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pyi/rules/unused_private_type_definition.rs#L161). I think [`delete_stmt`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/fix/edits.rs#L38) may help with the implementation, with some examples of its usage [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pie/rules/duplicate_class_field_definition.rs#L104) and [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs#L181).

---

_Label `fixes` added by @ntBre on 2025-02-04 15:08_

---

_Label `good first issue` added by @ntBre on 2025-02-04 15:08_

---

_Label `help wanted` added by @ntBre on 2025-02-04 15:08_

---

_Closed by @AlexWaygood on 2025-02-06 18:08_

---
