```yaml
number: 15669
title: "[red-knot] Missing `invalid-assignment` diagnostics"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-01-22T10:42:11Z
updated_at: 2025-03-04T02:16:06Z
url: https://github.com/astral-sh/ruff/issues/15669
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Missing `invalid-assignment` diagnostics

---

_@sharkdp_

### Description

There are some cases where we fail to issue `invalid-assignment` diagnostics. It seems like a good idea to increase our test coverage in this area. For example:

```py
class Foo: ...

# error: [invalid-assignment] "Implicit shadowing of class `Foo` …"
Foo = 1

# no error here
for Foo in [1, 2]: pass

# no error here
with some_context_manager() as Foo: pass

# no error here
type Foo = int

# no error here
import sys as Foo

...
```

---

_Label `bug` added by @sharkdp on 2025-01-22 10:42_

---

_Label `red-knot` added by @sharkdp on 2025-01-22 10:42_

---

_Comment by @carljm on 2025-03-03 21:51_

I think all of these examples are expected and "working as designed" according to our current model (and known type inference TODOs).

The iteration example is because we don't yet support generics, so we infer a `Todo` (dynamic) type for iterating over `[1, 2]`, and that dynamic type is assignable to anything. If we create a known iterable type and iterate over it, we do emit an `invalid-assignment` error here.

The context manager example is because `some_context_manager` doesn't exist, so again we infer `Unknown`, which is assignable to anything. If we use a real context manager class instead, we do emit an `invalid-assignment` error here as well.

The last two examples are both declarations themselves (we treat both `type` statements and imports as declarations), so since we permit re-declarations, those are both permitted "re-declarations" of the name `Foo`.

There are some decisions here that we may want to revisit because they maybe don't lead to intuitive behavior (as evidenced by the very existence of this issue). For example:

1) I'm not sure the special message for "shadowing a class" is useful. It might be better just to go with the usual `invalid-assignment` error message, plus a clearer display representation of class-literal types than `Literal[Foo]`. There are some issues with the heuristic we currently use to emit this message.

2) I'm not sure that imports should be allowed to re-declare a name.

But I think that we should revisit these questions in the context of running red-knot on real-world codebases and seeing what issues crop up; I don't think it's a priority to revisit now.

---

_Closed by @carljm on 2025-03-03 21:51_

---

_Comment by @carljm on 2025-03-03 22:01_

I do think at some point we will probably want to create a dedicated diagnostic for anytime an _unused_ binding of a variable is shadowed, and in practice this will catch a lot of the problematic accidental shadowing cases.

---
