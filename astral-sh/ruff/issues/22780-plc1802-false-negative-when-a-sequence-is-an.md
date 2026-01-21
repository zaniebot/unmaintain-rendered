```yaml
number: 22780
title: PLC1802 false negative when a sequence is an attribute
type: issue
state: open
author: leandrobbraga
labels:
  - bug
  - rule
assignees: []
created_at: 2026-01-20T23:43:47Z
updated_at: 2026-01-21T00:33:28Z
url: https://github.com/astral-sh/ruff/issues/22780
synced_at: 2026-01-21T00:50:35Z
```

# PLC1802 false negative when a sequence is an attribute

---

_@leandrobbraga_

PLC1802 won’t trigger if the sequence is an attribute.

https://play.ruff.rs/f9f8f76b-c468-4f4f-a3b8-0bca5850daf1

# Example

By running
```console
$ echo 'class Fruits:
      fruits: list[str]

      def __init__(self, fruits: list[str]):
          self.fruits = fruits

  fruits_class = Fruits(["orange", "apple"])
  fruits_list = ["a", "b"]

  if len(fruits_class.fruits):
      ...

  if len(fruits_list):
      ...
  ' | cargo run -p ruff -- check --select PLC1802 -
```

We get this output:

```console
PLC1802 [*] `len(fruits_list)` used as condition without comparison
  --> -:13:4
   |
11 |     ...
12 |
13 | if len(fruits_list):
   |    ^^^^^^^^^^^^^^^^
14 |     ...
   |
help: Remove `len`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Comment by @leandrobbraga on 2026-01-21 00:00_

It seems related to ruff giving up on resolving `Expr::Attribute` in `From<&Expr> for ResolvedPythonType` [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_semantic/src/analyze/type_inference.rs#L348-L353).

---

_Label `rule` added by @amyreese on 2026-01-21 00:33_

---

_Label `bug` added by @amyreese on 2026-01-21 00:33_

---
