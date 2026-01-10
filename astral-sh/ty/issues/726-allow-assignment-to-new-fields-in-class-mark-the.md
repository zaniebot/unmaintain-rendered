```yaml
number: 726
title: Allow assignment to new fields in class, mark the field as bound
type: issue
state: closed
author: flexagoon
labels:
  - diagnostics
  - wish
assignees: []
created_at: 2025-06-29T21:36:31Z
updated_at: 2025-06-30T21:52:52Z
url: https://github.com/astral-sh/ty/issues/726
synced_at: 2026-01-10T02:07:36Z
```

# Allow assignment to new fields in class, mark the field as bound

---

_Issue opened by @flexagoon on 2025-06-29 21:36_

### Summary

Not sure how else to title this issue, since this is two issues in one which relate to the same usecase.

```python
class A:
    foo: int

class B:
    pass

def bar(p: A | B):
    p.foo = 4
    print(p.foo)
```

This produces two issues:
```
Object of type `Literal[4]` is not assignable to attribute `foo` on type `A | B` (invalid-assignment) [Ln 8, Col 5]
Attribute `foo` on type `A | B` is possibly unbound (possibly-unbound-attribute) [Ln 9, Col 11]
```

First of all, there should be a way to allow assignments like that via config.

But also, in this case, p.foo is guaranteed to be defined, so the `possibly-unbound-attribute` rule is a false positive.

### Version

_No response_

---

_Label `diagnostics` added by @carljm on 2025-06-30 19:37_

---

_Label `wish` added by @carljm on 2025-06-30 19:37_

---

_Comment by @carljm on 2025-06-30 19:37_

Emitting diagnostics on both of those lines is the intended behavior: mypy, pyright, and pyrefly all do the same.

We could emit a different error code than just `invalid-assignment` for `p.foo = 4`; something like `undefined-attribute`. Then in your config you could silence that error code if you choose.

For the second diagnostic, I think we could use our narrowing support to avoid a diagnostic here, but I don't think this is high priority. The use case here is pushing against the way the Python type system is intended to work (if you want to set/access an attribute `B.foo`, then that attribute should always exist on `B`), and no other Python type checker supports it.

(Side note: I don't really like the way we currently phrase the second diagnostic. It would be better to just say that the attribute `foo` doesn't exist on `B`.)

---

_Comment by @flexagoon on 2025-06-30 21:52_

Alright, my bad, turns out mypy does that as well, it just didn't show an error for me because the class was imported from a library which was excluded from typechecking.

---

_Closed by @flexagoon on 2025-06-30 21:52_

---
