```yaml
number: 427
title: Guard clause fails to narrow types of nested attributes
type: issue
state: closed
author: MVanderloo
labels: []
assignees: []
created_at: 2025-05-16T17:44:13Z
updated_at: 2025-05-16T17:57:02Z
url: https://github.com/astral-sh/ty/issues/427
synced_at: 2026-01-10T02:34:09Z
```

# Guard clause fails to narrow types of nested attributes

---

_Issue opened by @MVanderloo on 2025-05-16 17:44_

### Summary

This is a situation that both mypy and pyright appear to detect correctly, where ty raises warning[possibly-unbound-attribute]

This seems to be specifically for nested attributes.

```python
class A:
    x: int

class B:
    a: A | None


def print_x(b: B):
    if b.a is None:
        return

    print(b.a.x)     # Attribute `x` on type `A | None` is possibly unbound
```

### Version

0.0.1-alpha.3 (144a26d44 2025-05-15)

---

_Comment by @MVanderloo on 2025-05-16 17:46_

Additionally this is a situation that I would expect to work

```python
def print_xs(bs: list[B]):
    for b in bs:
        if b.a is None:
            continue

        print(b.a.x)      # Attribute `x` on type `A | None` is possibly unbound
```

---

_Closed by @MichaReiser on 2025-05-16 17:57_

---
