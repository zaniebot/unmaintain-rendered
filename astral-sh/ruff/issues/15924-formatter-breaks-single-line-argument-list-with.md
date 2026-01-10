```yaml
number: 15924
title: "Formatter: breaks single-line argument list with comments into one argument per line"
type: issue
state: closed
author: bersbersbers
labels: []
assignees: []
created_at: 2025-02-04T08:24:06Z
updated_at: 2025-02-04T08:44:18Z
url: https://github.com/astral-sh/ruff/issues/15924
synced_at: 2026-01-10T11:09:57Z
```

# Formatter: breaks single-line argument list with comments into one argument per line

---

_Issue opened by @bersbersbers on 2025-02-04 08:24_

### Description

This formatter change (in v0.9.4) is unexpected to me. Black does nothing to this code:

Input, and black's output:
```python
def foo(
    a: A, b: B, c: C, d: D, e: E, f: F, g: G, h: H, i: I, j: J, k: K, l: L, M: m
) -> None: ...


def bar(
    a: A, b: B, c: C, d: D, e: E, f: F, g: G, h: H, i: I, j: J, k: K  # noqa: N803
) -> None: ...
```

Ruff's Output:
```python
def foo(
    a: A, b: B, c: C, d: D, e: E, f: F, g: G, h: H, i: I, j: J, k: K, l: L, M: m
) -> None: ...


def bar(
    a: A,
    b: B,
    c: C,
    d: D,
    e: E,
    f: F,
    g: G,
    h: H,
    i: I,
    j: J,
    k: K,  # noqa: N803
) -> None: ...
```

---

_Closed by @MichaReiser on 2025-02-04 08:44_

---
