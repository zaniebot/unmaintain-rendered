```yaml
number: 2005
title: Mixed return types
type: issue
state: closed
author: laurensnaturalis
labels:
  - question
assignees: []
created_at: 2025-12-17T12:34:56Z
updated_at: 2025-12-17T14:49:21Z
url: https://github.com/astral-sh/ty/issues/2005
synced_at: 2026-01-12T15:54:26Z
```

# Mixed return types

---

_@laurensnaturalis_

### Question

When using the following code

```py
cache_batch_features: Array = feature_cache.get("features")[start:end]
```

Where `feature_cache.get` is a from the `zarr` package with signature

```py
def get(self, path: str, default: DefaultT | None = None) -> Array | Group | DefaultT | None:
```

I get 

```
Method `__getitem__` of type `bound method Group.__getitem__(path: str) -> Array | Group` cannot be called with key of type `slice[Any, Any, Any]` on object of type `Group`
```

Should I change this to  
```py
tmp_ = feature_cache.get("features")[start:end]
if isinstance(tmp_, Array):
    tmp_[start:end]
```
?

And if yes, is this a consequence of the design of the `.get` function of 'zarr'?

### Version

_No response_

---

_Label `question` added by @laurensnaturalis on 2025-12-17 12:34_

---

_Comment by @sharkdp on 2025-12-17 12:47_

> Should I change this to

Yes, you need to narrow the type somehow to exclude `None` (and possibly `DefaultT`, depending on what it specializes to). So it might be enough to do `if tmp_ is not None`.

> And if yes, is this a consequence of the design of the `.get` function of 'zarr'?

Yes. This function is annotated as possibly returning `None`, so this is something you need to exclude before subscripting the returned object.

---

_Comment by @sharkdp on 2025-12-17 14:49_

Closing this based on the comment in #2011, but feel free to comment here if it's not resolved.

---

_Closed by @sharkdp on 2025-12-17 14:49_

---
