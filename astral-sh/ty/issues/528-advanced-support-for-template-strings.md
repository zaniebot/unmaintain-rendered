```yaml
number: 528
title: advanced support for template strings
type: issue
state: open
author: InSyncWithFoo
labels:
  - feature
  - typing semantics
assignees: []
created_at: 2025-05-28T03:43:14Z
updated_at: 2025-11-18T16:10:28Z
url: https://github.com/astral-sh/ty/issues/528
synced_at: 2026-01-12T15:54:23Z
```

# advanced support for template strings

---

_@InSyncWithFoo_

[PEP 750](https://peps.python.org/pep-0750/) introduces <i>template strings</i>, also known as t-strings:

```python
template: Template = t'Hello {name}'
```

According to the PEP, this feature is expected to be used for "safety checks, web templating, domain-specific languages, and more".

---

Obviously, this is a big feature and needs some design, but here are a few ideas to get things started:

```python
type_checker = 'ty'                   # Literal['ty']
template = t'{type_checker} is fast'  # LiteralTemplate[t'{type_checker} is fast', type_checker: Literal['ty']]

reveal_type(template.strings)         # tuple[Literal[''], 'is fast']
reveal_type(template.interpolations)  # tuple[LiteralInterpolation[Literal['ty']]]
```

---

_Comment by @AlexWaygood on 2025-05-28 09:23_

See also the discussion at https://github.com/python/cpython/issues/133970.

I'm not sure I'd want to introduce any kind of dedicated `Type` variant or special form for template strings until I'm a bit clearer on what the use cases are, and/or whether there's going to be any attempt to standardise support for generic `Template`s in typeshed. It's generally good to keep special-casing (and complexity in our model) to a minimum.

---

_Label `feature` added by @carljm on 2025-05-28 17:37_

---

_Label `typing semantics` added by @MichaReiser on 2025-11-14 09:16_

---

_Comment by @carljm on 2025-11-14 14:39_

We do now have basic support for template strings in 3.14, but it's just what the typeshed annotations give; there's no support for a "template literal" type that actually knows its details.

---

_Renamed from "PEP 750 support" to "advanced support for template strings" by @carljm on 2025-11-14 14:40_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:40_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
