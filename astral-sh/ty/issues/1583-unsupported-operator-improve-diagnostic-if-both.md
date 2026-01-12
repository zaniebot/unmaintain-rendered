```yaml
number: 1583
title: "`unsupported-operator`: Improve diagnostic if both sides have the same type"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-11-18T13:57:44Z
updated_at: 2025-12-19T22:12:59Z
url: https://github.com/astral-sh/ty/issues/1583
synced_at: 2026-01-12T15:54:25Z
```

# `unsupported-operator`: Improve diagnostic if both sides have the same type

---

_@MichaReiser_

```py
list[1] - list[2]
```

```
Operator `-` is unsupported between objects of type `<class 'list[Unknown]'>` and `<class 'list[Unknown]'>` (unsupported-operator) [Ln 1, Col 1]
```

Repeating the type here is not only verbose, but I also wondered if there's something I'm missing and that the types are indeed different in

```
error[unsupported-operator]: Operator `-` is unsupported between objects of type `_R@ignore_variance` and `_R@ignore_variance`
  --> homeassistant/util/variance.py:41:43
   |
39 |         value = func(*args, **kwargs)
40 |
41 |         if last_value is not None and abs(value - last_value) < ignored_variance:
   |                                           ^^^^^^^^^^^^^^^^^^
42 |             return last_value
   |
info: rule `unsupported-operator` is enabled by default
```

I'm not sure if this even allows us to optimize the operator lookup code to only lookup the dunder methods once rather than trying to do it on both types (but maybe we already do this?)

---

_Label `help wanted` added by @MichaReiser on 2025-11-18 13:57_

---

_Label `diagnostics` added by @MichaReiser on 2025-11-18 13:57_

---

_Renamed from "`unsupported-operator`: Improve diagnostic if the both sides have the same type" to "`unsupported-operator`: Improve diagnostic if both sides have the same type" by @sharkdp on 2025-11-18 14:00_

---

_Added to milestone `Stable` by @carljm on 2025-11-18 17:16_

---

_Comment by @Hugo-Polloli on 2025-12-19 22:10_

Started looking into it, but looks like it's already fixed by https://github.com/astral-sh/ruff/pull/21947 ?

---

_Comment by @AlexWaygood on 2025-12-19 22:12_

Yes, thank you, this is now fixed!

---

_Closed by @AlexWaygood on 2025-12-19 22:12_

---
