```yaml
number: 7324
title: "`deprecated-import` (`UP035`) - no error when importing types from `typing_extensions` that have always been in `typing`"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2023-09-12T23:39:05Z
updated_at: 2023-09-13T17:21:13Z
url: https://github.com/astral-sh/ruff/issues/7324
synced_at: 2026-01-10T11:09:49Z
```

# `deprecated-import` (`UP035`) - no error when importing types from `typing_extensions` that have always been in `typing`

---

_Issue opened by @DetachHead on 2023-09-12 23:39_

`typing_extensions` has a bunch of re-exported types that have alays been in `typing`:
```py
# Aliases for items that have always been in typing.
# Explicitly assign these (rather than using `from typing import *` at the top),
# so that we get a CI error if one of these is deleted from typing.py
# in a future version of Python
AbstractSet = typing.AbstractSet
AnyStr = typing.AnyStr
BinaryIO = typing.BinaryIO
Callable = typing.Callable
Collection = typing.Collection
Container = typing.Container
Dict = typing.Dict
ForwardRef = typing.ForwardRef
FrozenSet = typing.FrozenSet
Generator = typing.Generator
Generic = typing.Generic
Hashable = typing.Hashable
IO = typing.IO
ItemsView = typing.ItemsView
Iterable = typing.Iterable
Iterator = typing.Iterator
KeysView = typing.KeysView
List = typing.List
Mapping = typing.Mapping
MappingView = typing.MappingView
Match = typing.Match
MutableMapping = typing.MutableMapping
MutableSequence = typing.MutableSequence
MutableSet = typing.MutableSet
Optional = typing.Optional
Pattern = typing.Pattern
Reversible = typing.Reversible
Sequence = typing.Sequence
Set = typing.Set
Sized = typing.Sized
TextIO = typing.TextIO
Tuple = typing.Tuple
Union = typing.Union
ValuesView = typing.ValuesView
cast = typing.cast
no_type_check = typing.no_type_check
no_type_check_decorator = typing.no_type_check_decorator
```

ruff does not seem to show an error if you import any of these:
```py
from typing_extensions import cast # no error
```

---

_Label `bug` added by @charliermarsh on 2023-09-13 13:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-13 16:42_

---

_Closed by @charliermarsh on 2023-09-13 17:21_

---
