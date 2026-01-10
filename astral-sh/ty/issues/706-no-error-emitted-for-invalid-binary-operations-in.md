```yaml
number: 706
title: No error emitted for invalid binary operations in type expressions
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - typing semantics
assignees: []
created_at: 2025-06-25T19:23:33Z
updated_at: 2025-06-30T08:06:02Z
url: https://github.com/astral-sh/ty/issues/706
synced_at: 2026-01-10T02:07:36Z
```

# No error emitted for invalid binary operations in type expressions

---

_Issue opened by @AlexWaygood on 2025-06-25 19:23_

### Summary

We don't emit a diagnostic for this, but we should:

```py
x: 1 + 2
```

This bit of the code needs to be edited: https://github.com/astral-sh/ruff/blob/15f370e19c6bcaefd662a968b8a14c59dbbb36a6/crates/ty_python_semantic/src/types/infer.rs#L8366-L8370

it needs to look more like this: https://github.com/astral-sh/ruff/blob/15f370e19c6bcaefd662a968b8a14c59dbbb36a6/crates/ty_python_semantic/src/types/infer.rs#L8387-L8393

though we might have to do a tedious match over all the various kinds of binary operations in order to get good error messages.

### Version

_No response_

---

_Label `help wanted` added by @AlexWaygood on 2025-06-25 19:23_

---

_Label `typing semantics` added by @AlexWaygood on 2025-06-25 19:23_

---

_Comment by @med1844 on 2025-06-26 04:05_

Can I work on this? I can only code after work, so it might take a little bit longer, but PR should be ready within 2 days.

---

_Comment by @dhruvmanila on 2025-06-26 04:25_

@med1844 Sure!

---

_Assigned to @med1844 by @dhruvmanila on 2025-06-26 04:25_

---

_Comment by @med1844 on 2025-06-27 15:52_

After adding the diagnostic, I found that it breaks [this test](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md?plain=1#L254).

I'm trying to decide on the best approach to fix this. Should I:

1. replace the expression with `Intersection[LiteralString, Not[Literal[""]]]`?
2. add `#[invalid-type-form]`?
3. or do something else?

Thanks in advance for any input on this!

---

_Comment by @AlexWaygood on 2025-06-27 15:55_

@med1844 option (1) is the correct one there! That test is currently invalid; we don't support using `~` to express negated types (yet).

---

_Closed by @sharkdp on 2025-06-30 08:06_

---
