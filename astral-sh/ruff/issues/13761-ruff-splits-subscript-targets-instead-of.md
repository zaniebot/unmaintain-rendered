```yaml
number: 13761
title: Ruff splits subscript targets instead of parenthesizing the value.
type: issue
state: open
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-10-15T13:20:11Z
updated_at: 2025-11-24T15:09:54Z
url: https://github.com/astral-sh/ruff/issues/13761
synced_at: 2026-01-12T15:54:53Z
```

# Ruff splits subscript targets instead of parenthesizing the value.

---

_@MichaReiser_

```python
my_object.elemnt[
    0
].subelement = SomeEnumName.VERY_VERY_VERY_LONG_ENUM_VALUE_DESCeeeeeebbbdddddddddddddddddeeeddddd
```

This is ugly :sweat: 

Black does a better job and formats it as:

```python
my_object.elemnt[0].subelement = (
	SomeEnumName.VERY_VERY_VERY_LONG_ENUM_VALUE_DESCeeeeeebbbdddddddddddddddddeeeddddd
)
```

Which would align with how subscripts are handled in `can_omit_optional_parentheses`:

https://github.com/astral-sh/ruff/blob/0a86ffe9a5f5f9ca38d39ff2d1d2504804769fc5/crates/ruff_python_formatter/src/expression/mod.rs#L611-L612

---

_Label `bug` added by @MichaReiser on 2024-10-15 13:20_

---

_Label `formatter` added by @MichaReiser on 2024-10-15 13:20_

---

_Comment by @MichaReiser on 2025-11-24 15:09_

Note, that Ruff does format subscript correctly when using more than one assignment target (in which case the formatter takes the `RightToLeft` formatting target (which prefers breaking the right side over the left side). 

https://play.ruff.rs/e23c062b-3db3-412c-9c8f-f134f26404cb

---
