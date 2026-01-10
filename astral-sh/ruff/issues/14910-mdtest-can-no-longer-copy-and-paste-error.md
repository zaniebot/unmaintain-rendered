---
number: 14910
title: "mdtest: can no longer copy and paste error messages directly into mdtests"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - testing
  - ty
assignees: []
created_at: 2024-12-11T12:59:50Z
updated_at: 2024-12-11T13:44:28Z
url: https://github.com/astral-sh/ruff/issues/14910
synced_at: 2026-01-10T01:22:55Z
---

# mdtest: can no longer copy and paste error messages directly into mdtests

---

_Issue opened by @AlexWaygood on 2024-12-11 12:59_

I ran an mdtest locally, and it failed with this output:

```
  crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md:142 unmatched assertion: error: [lint:invalid-base] "Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)"
```

Okay. I copied and pasted the error message straight from my terminal, adding ``# error: [lint:invalid-base] "Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)"`` above the offending line in the mdtest to make it pass. I ran the mdtest again, and now it said:

```
  crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md:142 unmatched assertion: error: [lint:invalid-base] "Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)"
  crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md:142 unexpected error: 11 [lint:invalid-base] "Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)"
```

Note that the "expected message" there is identical to the message that mdtest said it got, but the test still failed! What mdtest _actually_ wants me to do to the mdtest to make the test pass is this:

```diff
- # error: [lint:invalid-base] "Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)"
+ # error: [invalid-base] "Invalid class base with type `GenericAlias` (all bases must be a class, `Any`, `Unknown` or `Todo`)"
```

But there's nothing in the error message it's giving me that would indicate that. This is very confusing UX :-)

Cc. @MichaReiser -- I think this was caused by one of your recent changes to diagnostics infrastructure in red-knot

---

_Label `bug` added by @AlexWaygood on 2024-12-11 12:59_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-11 12:59_

---

_Label `testing` added by @AlexWaygood on 2024-12-11 12:59_

---

_Referenced in [astral-sh/ruff#14914](../../astral-sh/ruff/pulls/14914.md) on 2024-12-11 13:17_

---

_Closed by @MichaReiser on 2024-12-11 13:37_

---

_Closed by @MichaReiser on 2024-12-11 13:37_

---

_Comment by @AlexWaygood on 2024-12-11 13:44_

Thanks @MichaReiser!

---
