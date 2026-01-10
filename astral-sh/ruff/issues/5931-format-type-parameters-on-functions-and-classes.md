```yaml
number: 5931
title: Format type parameters on functions and classes
type: issue
state: closed
author: konstin
labels:
  - formatter
  - python312
assignees: []
created_at: 2023-07-20T21:25:42Z
updated_at: 2023-08-24T20:02:28Z
url: https://github.com/astral-sh/ruff/issues/5931
synced_at: 2026-01-10T11:09:48Z
```

# Format type parameters on functions and classes

---

_Issue opened by @konstin on 2023-07-20 21:25_

[PEP 695](https://peps.python.org/pep-0695/) introduces type parameters on function and classes in python 3.12, which are currently not considered when formatting, but should be added:

```python
class ClassA[T: str]:
    def method1(self) -> T:
        ...

def func[T](a: T, b: T) -> T:
    ...
```

Their implementation can likely reuse larger parts of dict comprehensions. See https://github.com/psf/black/pull/3703 for the black implementation


---

_Label `formatter` added by @konstin on 2023-07-20 21:25_

---

_Assigned to @zanieb by @konstin on 2023-07-20 21:26_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 16:26_

---

_Closed by @zanieb on 2023-08-02 20:29_

---

_Label `python312` added by @zanieb on 2023-08-24 20:02_

---
