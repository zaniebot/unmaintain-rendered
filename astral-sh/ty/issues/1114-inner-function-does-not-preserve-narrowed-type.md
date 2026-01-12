```yaml
number: 1114
title: Inner function does not preserve narrowed type
type: issue
state: closed
author: mflova
labels:
  - narrowing
  - control flow
assignees: []
created_at: 2025-09-01T10:30:30Z
updated_at: 2025-09-02T17:38:55Z
url: https://github.com/astral-sh/ty/issues/1114
synced_at: 2026-01-12T15:54:24Z
```

# Inner function does not preserve narrowed type

---

_@mflova_

### Summary

`Mypy` and `pyright` apparently do not have this problem. In my example below, `obj` from `inner_func` is guaranteed to be a type string. This can also be seen in the first `reveal_type`. However, it seems that `ty` is taking the type of `obj` to be the input argument one (`int | str`) instead of the narrowed down (`str`)

```py
from collections.abc import Mapping
from typing import Any
from typing_extensions import reveal_type


def func(obj: int | str) -> None:
    obj = str(obj) if isinstance(obj, int) else obj
    reveal_type(obj)  #  str

    def inner_func():
        reveal_type(obj)  # int | str
```

### Version

0.0.1-alpha.19

---

_Comment by @sharkdp on 2025-09-01 10:50_

Thank you for reporting this.

This is a known limitation that is documented in [these tests](https://github.com/astral-sh/ruff/blob/bbfcf6e111648052783bdb68ec71706fc69b714f/crates/ty_python_semantic/resources/mdtest/public_types.md?plain=1#L251-L299). We currently do not yet analyze the control flow in the outer function to the extend that it would allow us to infer that the narrowing inside `inner_func` is safe for all calls to it. For example, narrowing would be unsafe if the body of the `func` function would continue like this:
```py
    obj = 1
    inner_func()
```

---

_Label `narrowing` added by @sharkdp on 2025-09-01 10:50_

---

_Label `control flow` added by @sharkdp on 2025-09-01 10:50_

---

_Comment by @mtshiba on 2025-09-01 11:24_

astral-sh/ruff#19932 addresses this issue.

---

_Closed by @carljm on 2025-09-02 17:38_

---
