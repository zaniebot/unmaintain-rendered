```yaml
number: 22780
title: PLC1802 false negative when a sequence is an attribute
type: issue
state: open
author: leandrobbraga
labels:
  - bug
  - rule
  - type-inference
assignees: []
created_at: 2026-01-20T23:43:47Z
updated_at: 2026-01-21T14:49:37Z
url: https://github.com/astral-sh/ruff/issues/22780
synced_at: 2026-01-21T15:05:36Z
```

# PLC1802 false negative when a sequence is an attribute

---

_@leandrobbraga_

PLC1802 won’t trigger if the sequence is an attribute.

https://play.ruff.rs/74477bb6-7796-4884-927a-2d0a8f93e4e2

# Example

By running
```console
$ echo 'class Fruits:
    fruits: list[str]

    def __init__(self):
        self.fruits = ["apple", "orange"]

        if len(self.fruits):
            ...

fruits = ["apple", "orange"]
if len(fruits):
    ...' | cargo run -p ruff -- check --select PLC1802 -
```

We get this output, which is missing the `self.fruits` inside the Fruits class:

```console
PLC1802 [*] `len(fruits)` used as condition without comparison
  --> -:11:4
   |
10 | fruits = ["apple", "orange"]
11 | if len(fruits):
   |    ^^^^^^^^^^^
12 |     ...
   |
help: Remove `len`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Comment by @leandrobbraga on 2026-01-21 00:00_

It seems related to ruff giving up on resolving `Expr::Attribute` in `From<&Expr> for ResolvedPythonType` [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_semantic/src/analyze/type_inference.rs#L348-L353).

```rust
impl From<&Expr> for ResolvedPythonType {
    fn from(expr: &Expr) -> Self {
        match expr {
            // Primitives.
            Expr::Dict(_) => ResolvedPythonType::Atom(PythonType::Dict),
            Expr::DictComp(_) => ResolvedPythonType::Atom(PythonType::Dict),
            // (...)
            Expr::Lambda(_)
            | Expr::Await(_)
            | Expr::Yield(_)
            | Expr::YieldFrom(_)
            | Expr::Compare(_)
            | Expr::Call(_)
            | Expr::Attribute(_)
            | Expr::Subscript(_)
            | Expr::Starred(_)
            | Expr::Name(_)
            | Expr::Slice(_)
            | Expr::IpyEscapeCommand(_) => ResolvedPythonType::Unknown,
        }
    }
}
```

---

_Label `rule` added by @amyreese on 2026-01-21 00:33_

---

_Label `bug` added by @amyreese on 2026-01-21 00:33_

---

_Comment by @ntBre on 2026-01-21 14:03_

I think this would probably follow a code path more like `is_indirect_sequence`

https://github.com/astral-sh/ruff/blob/9f34b4634d407b4a2fd1257bfea65f5b4c0eacf0/crates/ruff_linter/src/rules/pylint/rules/len_test.rs#L130-L133

than `is_sequence`, which uses `ResolvedPythonType` because we have to resolve the definition and other assignments to the attribute. I think it would be very tricky to do that reliably without type inference.

---

_Label `type-inference` added by @ntBre on 2026-01-21 14:03_

---

_Comment by @leandrodamascena on 2026-01-21 14:49_

I had the same problem and was about to open a new issue, but I found this one. I just submitted a PR for review.

---
