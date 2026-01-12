```yaml
number: 2045
title: "Handle \"deep\" mutual typevar constraints"
type: issue
state: open
author: dcreager
labels:
  - generics
assignees: []
created_at: 2025-12-18T00:42:15Z
updated_at: 2025-12-18T00:42:30Z
url: https://github.com/astral-sh/ty/issues/2045
synced_at: 2026-01-12T15:54:26Z
```

# Handle "deep" mutual typevar constraints

---

_@dcreager_

Pulling this out from https://github.com/astral-sh/ruff/pull/21551#discussion_r2566717765 into a standalone issue.

Now that we're using the new constraint solver for `Callable` types, we can create constraint sets with an interesting pattern that we don't yet support. The failing mdtest is:

```py
def invoke[A, B](fn: Callable[[A], B], value: A) -> B:
    return fn(value)

def head[T](xs: list[T]) -> T:
    return xs[0]

# TODO: this should be `Unknown | int`
reveal_type(invoke(head, [1, 2, 3]))
```

We end up inferring this constraint set when comparing `head` to `Callable[[A], B]`:

```
(B@invoke ≤ T@head) ∧ (list[T@head] ≤ A@invoke)
```

We then try to remove `T@head` from the constraint set by calculating

```
∃T@head ⋅ (B@invoke ≤ T@head) ∧ (list[T@head] ≤ A@invoke)
```

We should be able to pick `T@head = B@invoke` and simplify that to

```
(B@invoke = *) ∧ (list[B@invoke] ≤ A@invoke)
```

which would then be enough to propagate through the return type to discharge this TODO. This will require adding more derived facts to the sequent map.

For co- and contravariant uses of the typevar, this will be straightforward, because the subtyping will carry through monotonically. For invariant uses, we will probably need a new `Type` variant to encode/remember the constraints from the abstracted-away typevar.

---

_Label `generics` added by @dcreager on 2025-12-18 00:42_

---

_Assigned to @dcreager by @dcreager on 2025-12-18 00:42_

---

_Added to milestone `Stable` by @dcreager on 2025-12-18 00:42_

---
