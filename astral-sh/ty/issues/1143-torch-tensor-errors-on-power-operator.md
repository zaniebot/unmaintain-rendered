```yaml
number: 1143
title: "`torch.tensor` errors on power operator"
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2025-09-07T17:29:53Z
updated_at: 2025-10-14T12:27:54Z
url: https://github.com/astral-sh/ty/issues/1143
synced_at: 2026-01-12T15:54:24Z
```

# `torch.tensor` errors on power operator

---

_@janosh_

### Summary

```py
import torch

dr = torch.tensor([1.0, 2.0, 3.0])
dr**2 # error[unsupported-operator]: Operator `**` is unsupported between objects of type `Tensor` and `Literal[2]`
>>> tensor([1., 4., 9.])
```


```
uv pip show torch        
Using Python 3.13.0 environment at: /Users/janosh/.venv/py313
Name: torch
Version: 2.7.0
Location: /Users/janosh/.venv/py313/lib/python3.13/site-packages
Requires: filelock, fsspec, jinja2, networkx, setuptools, sympy, typing-extensions
```

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @sharkdp on 2025-09-08 07:08_

Thank you for reporting this. This is the same as https://github.com/astral-sh/ty/issues/908. #491 will fix this.

---

_Closed by @sharkdp on 2025-09-08 07:08_

---

_Closed by @sharkdp on 2025-10-14 12:27_

---
