---
number: 14740
title: "[red-knot] add narrowing support in match class patterns"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-12-02T18:30:12Z
updated_at: 2025-01-09T17:49:07Z
url: https://github.com/astral-sh/ruff/issues/14740
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] add narrowing support in match class patterns

---

_Issue opened by @carljm on 2024-12-02 18:30_

The goal of this issue is to make this mdtest pass:

```py
def get_bool() -> bool: ...
def get_object() -> object: ...
class A: ...
class B: ...

x = get_object()

reveal_type(x)  # revealed: object

match x:
    case A():
        reveal_type(x)  # revealed: A
    case B():
        reveal_type(x)  # revealed: B

reveal_type(x)  # revealed: object
```

This should just require filling in the TODO for `MatchClass` at https://github.com/astral-sh/ruff/blob/83651deac756db988f3809c21cff9b1535bef1c2/crates/red_knot_python_semantic/src/types/narrow.rs#L232

We don't need to worry about arguments yet; the goal here is just to narrow based on the implicit `isinstance` check performed by the class pattern.

See https://docs.python.org/3/reference/compound_stmts.html#class-patterns for the documentation of the language feature.

---

_Label `red-knot` added by @carljm on 2024-12-02 18:30_

---

_Assigned to @carljm by @carljm on 2024-12-02 18:30_

---

_Comment by @InSyncWithFoo on 2024-12-07 21:27_

I looked into this and got a major block:

```rust
match pattern.pattern(self.db).node() {
    // ...
    ast::Pattern::MatchClass(PatternMatchClass { cls, .. }) => {
        let inference = infer_scope_types(self.db, self.scope());  // This panics.
    }
    // ...
}
```

The last logged message is `Box<dyn Any>`.

The problem appears to be reproducible with just:

`````markdown
```py
x = object()

match x:
    case int():
        x  # Or any name
```
`````

[This comment](https://github.com/astral-sh/ruff/blob/269e47be961d3eb95c0351e8f7006b71e083a6a4/crates/red_knot_python_semantic/src/types/narrow.rs#L464) seems relevant:

```rust
// SAFETY: we should always have a symbol for every Name node.
```


---

_Comment by @carljm on 2024-12-09 21:20_

The inscrutable `Box<dyn Any>` error message indicates a Salsa query cycle. In this case it would be due to calling `infer_scope_types` in type narrowing, when in turn type narrowing may well have been called by `infer_scope_types` on that same scope.

For expressions that function as narrowing predicates, we will usually need to add them as standalone expressions when building the semantic index, so we can query their type independently. Though since match patterns don't use `Expr` but rather `ComparableExpr`, this will probably require adding some new infrastructure for standalone ComparableExpr.

(Note this task is assigned to me, it's not up-for-grabs at the moment.)

---

_Comment by @sharkdp on 2024-12-11 19:27_

Just a quick note that I touched some of the relevant code in #14759. So maybe contact me before starting with this task (in case #14759 has not been merged in the meantime).

---

_Assigned to @dcreager by @dcreager on 2024-12-31 14:41_

---

_Unassigned @carljm by @dcreager on 2024-12-31 14:41_

---

_Referenced in [astral-sh/ruff#15223](../../astral-sh/ruff/pulls/15223.md) on 2025-01-02 16:15_

---

_Closed by @carljm on 2025-01-09 17:49_

---
