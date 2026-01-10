```yaml
number: 346
title: "False positive with plotly.graph_objects.Scatter3d: \"No overload of bound method __init__ matches arguments\""
type: issue
state: closed
author: romainbourdain
labels: []
assignees: []
created_at: 2025-05-13T08:01:49Z
updated_at: 2025-05-13T08:40:48Z
url: https://github.com/astral-sh/ty/issues/346
synced_at: 2026-01-10T02:34:09Z
```

# False positive with plotly.graph_objects.Scatter3d: "No overload of bound method __init__ matches arguments"

---

_Issue opened by @romainbourdain on 2025-05-13 08:01_

### Summary

Hi,
I'm using Ty to type-check a project that uses plotly.graph_objects.Scatter3d, and I'm encountering what seems to be a false positive or a limitation in how Ty handles dynamic typing in libraries like Plotly.

Here's a code example:
```python
go.Scatter3d(
    x=x,
    y=y,
    z=z,
    marker=dict(size=4),
    text=labels,
    textposition="top center",
)
```

Ty returns the following error:
```
No overload of bound method `__init__` matches argumentsty(no-matching-overload)
<class 'dict'>
class dict(**kwargs: _VT)
class dict(map: Mapping[_KT, _VT], **kwargs: _VT)
class dict(iterable: Iterable[Tuple[_KT, _VT]], **kwargs: _VT)
dict() -> new empty dictionary
dict(mapping) -> new dictionary initialized from a mapping object's
    (key, value) pairs
dict(iterable) -> new dictionary initialized as if via:
    d = {}
    for k, v in iterable:
        d[k] = v
dict(**kwargs) -> new dictionary initialized with the name=value pairs
    in the keyword argument list.  For example:  dict(one=1, two=2)
Full name: builtins.dict
```

This code runs correctly, and other type checkers like mypy do not raise issues here.

### Version

_No response_

---

_Comment by @sharkdp on 2025-05-13 08:40_

Thank you for reporting this. As far as I can tell, this is not related to the construction of `plotly.graph_objects.Scatter3d` (which we understand just fine in my experiments), but to the `dict(size=4)` call, which we don't understand yet:
```py
dict(size=4)  # No overload of bound method `__init__` matches arguments
```

This is already tracked here: https://github.com/astral-sh/ty/issues/100

---

_Closed by @sharkdp on 2025-05-13 08:40_

---
