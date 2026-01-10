```yaml
number: 927
title: nonlocal snapshot sweeping considers unrelated scopes, sweeps too much
type: issue
state: open
author: oconnor663
labels: []
assignees: []
created_at: 2025-08-01T16:32:39Z
updated_at: 2025-08-01T16:33:59Z
url: https://github.com/astral-sh/ty/issues/927
synced_at: 2026-01-10T02:06:24Z
```

# nonlocal snapshot sweeping considers unrelated scopes, sweeps too much

---

_Issue opened by @oconnor663 on 2025-08-01 16:32_

As of https://github.com/astral-sh/ruff/pull/19321 we use "lazy snapshots" to track when a variable in an enclosing scope is never re-bound, which lets us infer a narrower type in nested functions. For example (working as intended):

```py
def foo():
    x: int = 1
    def bar():
        reveal_type(x)  # `Literal[1]`

def foo():
    x: int = 1
    def bar():
        reveal_type(x)  # `int` (un-narrowed because of reassignment)
    x = 2
```

Similarly, we sweep/ignore enclosing snapshots of a variable [if any nested function declares that variable `nonlocal` and binds it](https://github.com/astral-sh/ruff/blob/48d5bd13faf0def62afca7477dc2ec86b2f2ad69/crates/ty_python_semantic/src/semantic_index/builder.rs#L414-L440) (also working as intended):

```py
def foo():
    x: int = 1
    def bar():
        reveal_type(x)  # `int` (un-narrowed because of nonlocal assignment)
    def baz():
        nonlocal x
        x = 2
```

However, that sweeping is currently more aggressive than it needs to be. It finds `nonlocal`+bound symbols of the same name, even in unrelated later (not earlier) scopes:

```py
def foo():
    x: int = 1
    def bar():
        reveal_type(x)  # `int` (should be narrowed)

def bing():
    x = 2
    def baz():
        nonlocal x
        x = 3
```

Similarly, it finds symbols in nested scopes that potentially could be aliasing but actually aren't because of shadowing:

```py
def foo():
    x: int = 1
    def bar():
        reveal_type(x)  # `int` (should be narrowed)
    def bing():
        x = 2
        def baz():
            nonlocal x
            x = 3
```

I'm currently working on adding a couple maps to symbol tables that will track "reference -> definition in enclosing scope" and "definition -> references in nested scopes", and it might be easier to fix this once that change lands. I'll link it here when I put up a PR. cc @mtshiba

---
