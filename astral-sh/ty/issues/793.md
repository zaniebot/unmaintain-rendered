```yaml
number: 793
title: "`ty` incorrectly skips over definitions that are not bindings when checking not-local loads"
type: issue
state: closed
author: oconnor663
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-07-09T16:27:49Z
updated_at: 2025-07-16T15:47:08Z
url: https://github.com/astral-sh/ty/issues/793
synced_at: 2026-01-10T02:07:36Z
```

# `ty` incorrectly skips over definitions that are not bindings when checking not-local loads

---

_Issue opened by @oconnor663 on 2025-07-09 16:27_

https://play.ty.dev/9040c09e-01bb-4866-b4b5-9f5d49ea31d2

```py
def f():
    x = 1
    def g():
        x: int
        def h():
            y = x
        h()
    g()
f()
```

```
$ python test.py
Traceback (most recent call last):
  File "/tmp/test.py", line 10, in <module>
    f()
    ~^^
  File "/tmp/test.py", line 9, in f
    g()
    ~^^
  File "/tmp/test.py", line 8, in g
    h()
    ~^^
  File "/tmp/test.py", line 7, in h
    y = x
        ^
NameError: cannot access free variable 'x' where it is not associated with a value in enclosing scope
```

```
$ uvx ty check test.py
All checks passed!
```

`ty` currently skips over `x: int` in `g` and decides to load `x = 1` from `f`. My theory is that the comment here is correct, but the treatment of `.is_bound()` here isn't doing what the comment says it's doing: https://github.com/astral-sh/ruff/blob/1eff0300d3784d116a19909bf0c1a493de2aea61/crates/ty_python_semantic/src/types/infer.rs#L5935-L5950

---

_Comment by @oconnor663 on 2025-07-09 17:47_

Here's another kind of related case:

```py
x: int = 1
def f():
    x: str = "hello"
    def g():
        global x
        def h():
            y: int = x
            print(y)
        h()
    g()
f()
```

```
$ python test.py
1
```

```
$ uvx ty check test.py
error[invalid-assignment]: Object of type `str` is not assignable to `int`
 --> test.py:7:13
  |
5 |         global x
6 |         def h():
7 |             y: int = x
  |             ^
8 |             print(y)
9 |         h()
  |
info: rule `invalid-assignment` is enabled by default
```

---

_Label `bug` added by @AlexWaygood on 2025-07-09 17:48_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-07-09 17:48_

---

_Comment by @oconnor663 on 2025-07-11 20:59_

Ok the first case is trickier than I thought, because you might have something like this:
```python
def f():
    x: int
    def g1():
        return x
    def g2():
        nonlocal x
        x = 2
    g2()
    g1()
f()
```
This isn't a name error, because `g2` is called before `g1`. To avoid a false positive here, we'd need to track that `x` is `nonlocal` in a _sibling_ inner scope (and possibly ask whether a binding is made in that scope). It looks like Pyright does do that analysis today, but afaik Ty doesn't. So I don't think it's necessarily a bug that the first case above isn't an error. However, it _is_ a bug that the reported type is wrong:

```py
x: int = 1
def f():
    x: str
    def g():
        reveal_type(x)  # should be `str`, is currently `int`
```

---

_Comment by @oconnor663 on 2025-07-14 15:11_

Fixing the second bug is uncovering a third semi-related false positive in current `main`:

```py
def f():
    global x
    x = 5
def g():
    print(x)
```

```
error[unresolved-reference]: Name `x` used when not defined
 --> test2.py:5:11
  |
3 |     x = 5
4 | def g():
5 |     print(x)
  |           ^
  |
```

Edit: @AlexWaygood points out that this error might be intentional/preferable, even though the code works at runtime.

---

_Comment by @oconnor663 on 2025-07-16 15:47_

Note that we still don't check whether variables in enclosing scopes are ever _bound_, e.g.:

```py
x: int
def f():
    y = x  # Ty doesn't lint here.
```

So for that reason, the first example at the top of this issue still doesn't lint. But now that https://github.com/astral-sh/ruff/pull/19336 is in, it is looking at the right variable, and we can see that by adding type declarations:

```py
x = "hello"
def f():
    x: int
    def g():
        y: int = x  # previously invalid assignment, now allowed (even though this `x` is unbound)
```

---

_Closed by @oconnor663 on 2025-07-16 15:47_

---
