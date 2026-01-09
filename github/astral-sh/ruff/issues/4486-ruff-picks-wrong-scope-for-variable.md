---
number: 4486
title: Ruff picks wrong scope for variable
type: issue
state: closed
author: notatallshaw
labels:
  - bug
assignees: []
created_at: 2023-05-18T02:04:26Z
updated_at: 2023-05-18T14:30:03Z
url: https://github.com/astral-sh/ruff/issues/4486
synced_at: 2026-01-07T13:12:14-06:00
---

# Ruff picks wrong scope for variable

---

_Issue opened by @notatallshaw on 2023-05-18 02:04_

Specifically, Ruff incorrectly identifies the scope a variable is chosen from when it's inside a comprehension inside a class inside a function and a variable with the same name exists inside the class scope.

Here is shortest code to reproduce this example, I named it `scopetest.py`:

```python
def f():
    b = 'Function Scope'
    class C:
        b = 'Class Scope'
        d = [b for _ in range(1)]
    return C

print(f().d[0])  # Prints "Function Scope"
```

Running Ruff:

```bash
> ruff --version
ruff 0.0.267
> ruff scopetest.py
scopetest.py:2:5: F841 [*] Local variable `b` is assigned to but never used
Found 1 error.
```

If I run `ruff` with `--fix` it deletes the line that b is sourced from causing the code to now throw an exception.

The reason b is scoping from the function scope and not the class scope is that generators / comprehensions implicitly create an anonymous function and source their scope from locals, non-locals, globals, and builtins and skip the class scope as all other functions do.

I came across this testing different versions of code [posted here](https://discuss.python.org/t/pep-709-one-behavior-change-that-was-missed-in-the-pep/26691) where the Steering Council has decided to keep the behavior as-is for at least Python 3.12.

---

_Label `bug` added by @charliermarsh on 2023-05-18 02:09_

---

_Comment by @charliermarsh on 2023-05-18 02:10_

Interesting, thank you for filing.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-18 03:04_

---

_Comment by @charliermarsh on 2023-05-18 03:08_

Do you understand why this runs without error?

```py
class A:
    T = range(10)

    Z = (x for x in T)
    L = [x for x in T]
    B = dict((i, str(i)) for i in T)
```

---

_Comment by @charliermarsh on 2023-05-18 03:18_

Ah, so this scoping only applies to the generator body, and not the iterable.

---

_Comment by @notatallshaw on 2023-05-18 03:20_

> Ah, so this scoping only applies to the generator body, and not the iterable.

Yes, your example initially threw me off, but I think in this case the iterable can be thought of like the function signature (e.g. the defaults of the arguments), it's scope is in the outer area and the body is like a function body.

---

_Comment by @charliermarsh on 2023-05-18 03:23_

Here's another interesting wrinkle -- this does _not_ work:

```py
class A:
    T = range(10)

    L = [x for x in T for y in T]
 ```

I noticed this via the comment in Carl Meyer's [finder tool](https://github.com/carljm/compfinder/blob/8dabc1d256431b1b2bd4c578a28d38f4ccad0ea0/finder.py#L207).

---

_Comment by @charliermarsh on 2023-05-18 03:23_

So it's only the first generator (?) that's evaluated in the parent scope.

---

_Comment by @notatallshaw on 2023-05-18 03:30_

> So it's only the first generator (?) that's evaluated in the parent scope.

Right, expanding my analogy same how the signature of bar fails here:

```python
class A:
    T = range(10)

    def foo(x=T):
        def bar(y=T):
            pass
        return bar()
    foo()
```


---

_Comment by @charliermarsh on 2023-05-18 03:31_

Thank you, that's a really helpful model.

---

_Referenced in [astral-sh/ruff#4494](../../astral-sh/ruff/pulls/4494.md) on 2023-05-18 14:13_

---

_Closed by @charliermarsh on 2023-05-18 14:30_

---
