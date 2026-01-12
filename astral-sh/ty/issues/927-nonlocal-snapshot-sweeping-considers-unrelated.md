```yaml
number: 927
title: nonlocal snapshot sweeping considers unrelated scopes, sweeps too much
type: issue
state: open
author: oconnor663
labels: []
assignees: []
created_at: 2025-08-01T16:32:39Z
updated_at: 2026-01-10T18:01:40Z
url: https://github.com/astral-sh/ty/issues/927
synced_at: 2026-01-12T15:54:24Z
```

# nonlocal snapshot sweeping considers unrelated scopes, sweeps too much

---

_@oconnor663_

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

_Comment by @udifuchs on 2026-01-10 17:55_

This issue bit me in an annoying way. My tests have code which is equivalent to:
```py
def test() -> None:
    x = "hello"
    y: int = 1

    def inner_test() -> None:
        nonlocal x
        x = "world"

    inner_test()
    assert x == "world"

    y = "2"  # Should be: Object of type `Literal["2"]` is not assignable to `int`
```
`ty` assumes that `x` does not change and therefore treats all code after the `assert` as unreachable.
This causes `ty` to silently ignore all type errors after the `assert`.

There is a PR to fix this issue in astral-sh/ruff#19820.  But it seems dormant.
I think that issue deserves a milestone tag.

---
