```yaml
number: 20851
title: "`is_mutable_expr` causes false negatives in B039, RUF008, RUF012, and RUF024"
type: issue
state: open
author: dscorbett
labels:
  - rule
  - preview
assignees: []
created_at: 2025-10-13T20:38:02Z
updated_at: 2025-10-15T14:26:27Z
url: https://github.com/astral-sh/ruff/issues/20851
synced_at: 2026-01-10T11:09:59Z
```

# `is_mutable_expr` causes false negatives in B039, RUF008, RUF012, and RUF024

---

_Issue opened by @dscorbett on 2025-10-13 20:38_

### Summary

The same issue as in #20004 affects [`mutable-contextvar-default` (B039)](https://docs.astral.sh/ruff/rules/mutable-contextvar-default/), [`mutable-dataclass-default` (RUF008)](https://docs.astral.sh/ruff/rules/mutable-dataclass-default/), [`mutable-class-default` (RUF012)](https://docs.astral.sh/ruff/rules/mutable-class-default/), and [`mutable-fromkeys-value` (RUF024)](https://docs.astral.sh/ruff/rules/mutable-fromkeys-value/). The extra cases in `is_guaranteed_mutable_expr` should be merged into `is_mutable_expr`. [Examples](https://play.ruff.rs/711a14ce-bcbf-4d7d-8a98-49bafc373d99):
```python
from contextvars import ContextVar
ContextVar("cv", default=(x for x in "cv"))

from dataclasses import dataclass
@dataclass
class D:
    x: list[int] | object = ([],)

class C:
    x: list[object] = (x := [])
    y = (y := [])

dict.fromkeys(["A", "B"], ([],))
```

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Label `bug` added by @ntBre on 2025-10-15 14:25_

---

_Label `bug` removed by @ntBre on 2025-10-15 14:25_

---

_Label `rule` added by @ntBre on 2025-10-15 14:25_

---

_Comment by @ntBre on 2025-10-15 14:26_

It looks like  these are all stable rules, so we'll need to make these preview changes like in https://github.com/astral-sh/ruff/pull/20024.

---

_Label `preview` added by @ntBre on 2025-10-15 14:26_

---
