```yaml
number: 474
title: "False positive too-many-positional-arguments on Protocols with ParamSpec for Python >= 3.12"
type: issue
state: closed
author: IbarraSamuel
labels:
  - generics
assignees: []
created_at: 2025-05-21T17:29:57Z
updated_at: 2025-05-30T01:07:14Z
url: https://github.com/astral-sh/ty/issues/474
synced_at: 2026-01-12T15:54:23Z
```

# False positive too-many-positional-arguments on Protocols with ParamSpec for Python >= 3.12

---

_@IbarraSamuel_

### Summary

when using a Protocol with ParamSpec in a class, ty doesn't allow to specify the ParamSpec for the protocol, and fails saying that the protocol is not expecting arguments.
see:
```python3
from typing import Protocol


class MyClass[**P](Protocol):
    def return_arg(self, *args: P.args, **kwargs: P.kwargs) -> None: ...


def my_func[**P2](x: MyClass[P2], *args: P2.args, **kwargs: P2.kwargs) -> None:  #  Too many positional arguments to class `MyClass`: expected 0, got 1 ty[too-many-positional-arguments](https://ty.dev/rules#too-many-positional-arguments)
    return x.return_arg(*args, **kwargs)

```



### Version

ty 0.0.1-alpha.6

---

_Label `calls` added by @AlexWaygood on 2025-05-21 17:30_

---

_Comment by @IbarraSamuel on 2025-05-21 17:32_

Simplified version.
```python3
from typing import Protocol


class MyClass[**P](Protocol):
    pass


def my_func[**P2](_x: MyClass[P2]):
    pass
```

---

_Label `calls` removed by @AlexWaygood on 2025-05-21 17:36_

---

_Label `generics` added by @AlexWaygood on 2025-05-21 17:36_

---

_Comment by @carljm on 2025-05-21 19:45_

Thanks for the report. We don't support `ParamSpec` yet, but we should soon, which will fix this.

---

_Comment by @carljm on 2025-05-30 01:07_

Closing as duplicate of #157 -- thanks for the report!

---

_Closed by @carljm on 2025-05-30 01:07_

---
