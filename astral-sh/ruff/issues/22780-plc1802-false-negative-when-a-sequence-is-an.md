```yaml
number: 22780
title: PLC1802 false negative when a sequence is an attribute
type: issue
state: open
author: leandrobbraga
labels: []
assignees: []
created_at: 2026-01-20T23:43:47Z
updated_at: 2026-01-20T23:45:53Z
url: https://github.com/astral-sh/ruff/issues/22780
synced_at: 2026-01-20T23:51:32Z
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
