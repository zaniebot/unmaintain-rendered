---
number: 2253
title: Unable to infer zip type
type: issue
state: closed
author: phistep
labels:
  - question
  - generics
  - Protocols
assignees: []
created_at: 2025-12-29T02:18:39Z
updated_at: 2025-12-29T14:23:11Z
url: https://github.com/astral-sh/ty/issues/2253
synced_at: 2026-01-10T01:51:14Z
---

# Unable to infer zip type

---

_Issue opened by @phistep on 2025-12-29 02:18_

### Question

```py
class Foo: ...


d = dict(a=Foo())

p = list(d.items())
pp = zip(p, p, strict=True)
```

<img width="429" height="183" alt="Image" src="https://github.com/user-attachments/assets/bbc41460-b8ed-42c2-b2fd-16398187eda6" />

https://play.ty.dev/7c1cf9a0-a632-41bc-a7ac-552c87e34308

I would expect the type checker to be able to infer the type of `pp` fully, as it knows the type of `p`. what would the correct type be here?

`zip` has a signature of 

```py
def __new__(
    iter1: Iterable[_T1@__new__],
    iter2: Iterable[_T2@__new__],
    *,
    /,
    strict: bool = False
) -> zip[tuple[_T1@__new__, _T2@__new__]]
```

so my guess would be

```py
zip[
    tuple[
        tuple[str, Foo]
        tuple[str, Foo]
    ]
]
```

### Version

```
~/Library/Application Support/Zed/languages/ty/ty-0.0.7/ty-aarch64-apple-darwin/ty
```

Bundled with Zed `0.217.3+stable.105.80433cb239e868271457ac376673a5f75bc4adb1`

---

_Label `question` added by @phistep on 2025-12-29 02:18_

---

_Comment by @MichaReiser on 2025-12-29 10:35_

I might be wrong on this (not a typing expert) but I think this might be due to ty's lack of support for generic protocols (`Iterable` is a generic protocol).

See https://github.com/astral-sh/ty/issues/1714

---

_Label `generics` added by @MichaReiser on 2025-12-29 10:35_

---

_Label `Protocols` added by @MichaReiser on 2025-12-29 10:35_

---

_Comment by @AlexWaygood on 2025-12-29 14:23_

Yes, this looks like #1714

---

_Closed by @AlexWaygood on 2025-12-29 14:23_

---
