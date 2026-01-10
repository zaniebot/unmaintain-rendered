```yaml
number: 1525
title: "PEP 604 union syntax should be allowed in stubs (`.pyi`) on all Python versions"
type: issue
state: closed
author: Avasam
labels: []
assignees: []
created_at: 2025-11-11T19:23:46Z
updated_at: 2025-11-11T19:30:29Z
url: https://github.com/astral-sh/ty/issues/1525
synced_at: 2026-01-10T02:06:25Z
```

# PEP 604 union syntax should be allowed in stubs (`.pyi`) on all Python versions

---

_Issue opened by @Avasam on 2025-11-11 19:23_

### Summary

Using https://peps.python.org/pep-0604/ should be valid in a type stubs, even on older Python versions, but ty says no:

```
error[unsupported-operator]: Operator `|` is unsupported between objects of type `<class 'ComInterface'>` and `<class 'DispInterface'>`
   --> typings\comtypes\tools\typedesc.pyi:146:25
    |
145 | _ImplTypeFlags = int
146 | _Interface: TypeAlias = ComInterface | DispInterface
    |                         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
147 |
148 | class CoClass:
    |
info: Note that `X | Y` PEP 604 union syntax is only available in Python 3.10 and later
info: Python 3.9 was assumed when resolving types
  --> pyproject.toml:46:19
   |
44 |   "Topic :: Multimedia :: Graphics :: Capture :: Screen Capture",
45 | ]
46 | requires-python = ">= 3.9"
   |                   ^^^^^^^^ Python 3.9 assumed due to this configuration setting
47 | dependencies = [
48 |   "comtypes >= 1.1.7 ; python_version < '3.13'",
   |
info: rule `unsupported-operator` is enabled by default
```

MRE:
```toml
# pyproject.toml
[project]
requires-python = ">= 3.9"
```
```python
# example.pyi
from typing_extensions import TypeAlias
example: TypeAlias = str | int
```


### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Comment by @AlexWaygood on 2025-11-11 19:27_

heh, I fixed this four hours ago in https://github.com/astral-sh/ruff/pull/21379 😄 but thank you!

---

_Closed by @AlexWaygood on 2025-11-11 19:27_

---

_Comment by @Avasam on 2025-11-11 19:30_

I got sniped !

---
