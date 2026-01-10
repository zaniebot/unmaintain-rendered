```yaml
number: 183
title: support enums
type: issue
state: closed
author: carljm
labels:
  - runtime semantics
  - enums
assignees: []
created_at: 2025-03-11T23:49:42Z
updated_at: 2025-09-23T20:02:10Z
url: https://github.com/astral-sh/ty/issues/183
synced_at: 2026-01-10T02:06:24Z
```

# support enums

---

_Issue opened by @carljm on 2025-03-11 23:49_

Doc: https://docs.python.org/3/library/enum.html
Typing spec: https://typing.python.org/en/latest/spec/enums.html

Base features:

- [x] Differentiating members from non-members
- [x] Create a new type variant for enum literals
- [x] Enum literal expansion
- [x] Write a micro benchmark with large enums
- [x] Enforce implicit `@final` for Enum classes (with members)
- [x] Support iteration over enums
- [x] Detect enums in all cases (metaclass derives from `EnumMeta`/`EnumType`)

Advanced features:

- [ ] Infer type for [member names](https://typing.python.org/en/latest/spec/enums.html#member-names) and [member values](https://typing.python.org/en/latest/spec/enums.html#member-values).
- [ ] Handling of `enum.Flag` (enum expansion may not be performed for subclasses of `enum.Flag`)
- [ ] Special-case handling of `IntEnum`, `StrEnum`, or other custom classes that derive from both `Enum` and some other class (infer appropriate types for member values)
- [ ] Custom `__new__` or `__init__` methods in enums.
- [ ] `auto()` values
- [ ] Support for _generate_next_value_
- [ ] Special-case handling for call to enum class to retrieve member by value (e.g. Color(“red”))
- [ ] Function syntax to create Enums

---

_Renamed from "[red-knot] support enums" to "support enums" by @MichaReiser on 2025-05-07 15:26_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:29_

---

_Assigned to @sharkdp by @MichaReiser on 2025-07-22 09:33_

---

_Comment by @sharkdp on 2025-07-23 06:52_

We support all of the "basic" features of enums now, so I'm closing this ticket. The things listed as "advanced" are now part of a new ticket: https://github.com/astral-sh/ty/issues/876

---

_Closed by @sharkdp on 2025-07-23 06:52_

---

_Label `enums` added by @AlexWaygood on 2025-09-23 20:02_

---
