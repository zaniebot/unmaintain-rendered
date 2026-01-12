```yaml
number: 2200
title: "Conversion of `collections.abc.Collection[str] & ~AlwaysFalsy` to `set()` results in `set[Unknown]` instead of `set[str]`"
type: issue
state: open
author: sinon
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-12-24T09:28:59Z
updated_at: 2025-12-27T00:28:28Z
url: https://github.com/astral-sh/ty/issues/2200
synced_at: 2026-01-12T15:54:26Z
```

# Conversion of `collections.abc.Collection[str] & ~AlwaysFalsy` to `set()` results in `set[Unknown]` instead of `set[str]`

---

_@sinon_

### Summary

This builds upon the issue raised in:
https://github.com/astral-sh/ty/issues/2060

The MRE I supplied of the real-world code elided a Falsey check, which does seem to disrupt the fix that was made for https://github.com/astral-sh/ty/issues/2060

https://play.ty.dev/f980f01d-288a-48cc-a04a-759f80d74b9d
```py
from collections.abc import Collection
from typing import Mapping, reveal_type

def something(mapping: Collection[str] | None = None):
    if not mapping:
        raise ValueError
    reveal_type(mapping)  # Collection[str] & ~AlwaysFalsy
    mapping = set(mapping)
    reveal_type(mapping) # set[Unknown]
```


### Version

0.0.6

---

_Renamed from "Converion of `abc.Collection[str] & ~AlwaysFalsey` restults in `set[Unknown]` instead of `set[str]`" to "Converion of `abc.Collection[str] & ~AlwaysFalsey` results in `set[Unknown]` instead of `set[str]`" by @sinon on 2025-12-24 09:29_

---

_Renamed from "Converion of `abc.Collection[str] & ~AlwaysFalsey` results in `set[Unknown]` instead of `set[str]`" to "Converion of `abc.Collection[str] & ~AlwaysFalsey` to `set()` results in `set[Unknown]` instead of `set[str]`" by @sinon on 2025-12-24 09:31_

---

_Renamed from "Converion of `abc.Collection[str] & ~AlwaysFalsey` to `set()` results in `set[Unknown]` instead of `set[str]`" to "Conversion of `abc.Collection[str] & ~AlwaysFalsey` to `set()` results in `set[Unknown]` instead of `set[str]`" by @AlexWaygood on 2025-12-24 09:33_

---

_Renamed from "Conversion of `abc.Collection[str] & ~AlwaysFalsey` to `set()` results in `set[Unknown]` instead of `set[str]`" to "Conversion of `abc.Collection[str] & ~AlwaysFalsy` to `set()` results in `set[Unknown]` instead of `set[str]`" by @AlexWaygood on 2025-12-24 09:33_

---

_Renamed from "Conversion of `abc.Collection[str] & ~AlwaysFalsy` to `set()` results in `set[Unknown]` instead of `set[str]`" to "Conversion of `collections.abc.Collection[str] & ~AlwaysFalsy` to `set()` results in `set[Unknown]` instead of `set[str]`" by @AlexWaygood on 2025-12-24 09:34_

---

_Label `generics` added by @AlexWaygood on 2025-12-24 09:34_

---

_Label `bidirectional inference` added by @carljm on 2025-12-27 00:21_

---

_Added to milestone `Stable` by @carljm on 2025-12-27 00:28_

---
