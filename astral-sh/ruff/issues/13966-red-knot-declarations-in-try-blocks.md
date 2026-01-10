```yaml
number: 13966
title: "[red-knot] declarations in try blocks"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-10-28T16:58:55Z
updated_at: 2024-12-17T04:19:41Z
url: https://github.com/astral-sh/ruff/issues/13966
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] declarations in try blocks

---

_Issue opened by @carljm on 2024-10-28 16:58_

Type declarations via variable annotations in red-knot are control-flow sensitive. That is, if you have

```py
if flag:
    x: str = "foo"
else:
    x: int = 1
```

We will allow that code. But we currently error if you try to assign to a variable at a point in control flow that is reachable by multiple conflicting declarations:

```py
if flag:
    x: str = "foo"
else:
    x: int = 1

# Whether this assignment is valid or not is somewhat ambiguous,
# so we currently error on it with "conflicting declared types, int vs str".
# The error can be fixed by making this `x: int = 3` instead, which
# explicitly overrides the conflicting prior declarations with a new one.
# We could also just choose to stop emitting that error entirely, and
# allow assignment of either ints or strings here (declared type
# implicitly becomes `int | str` at the control flow join point.)
x = 3
```

We also consider "no declaration" (which is implicitly a declaration of `Unknown`, allowing all assignments to that variable) as a conflict with "some declaration", so this code is also currently an error:

```py
if flag:
    x: int = 1
x = 2  # conflicting declarations, `int` vs `Unknown`
```

Other options here would include just letting the declared type implicitly be `int | Unknown` after the end of the `if flag:` block, or ignoring undeclared paths and just having the post-if declared type be `int`.

The current behavior is particularly problematic with a `try` block. In this code:

```py
try:
    x: int = call_that_may_raise()
except:
    x = 2
```

Our understanding of control flow in `try` blocks accounts for the fact that we could enter the `except` block without having run some or any of the code in the `try` block (in case of an exception.) Since we reuse that same understanding of control flow for deciding where declarations reach, we end up seeing "conflicting declarations `int` vs `Unknown`" at the `x = 2` assignment, because we see that control flow can reach that point with or without having fully executed the line `x: int = call_that_may_raise()`.

This doesn't make intuitive sense, because type declarations are a static analysis concept, not a runtime concept, and control-flow jumps due to exceptions are implicit runtime behavior, not obvious in the code structure like other control-flow constructs. It would be better in this case if we considered the `x: int` declaration to be unconditional.

If we ignored undeclared paths (as discussed above), that would make the undeclared-path part of this problem disappear.

You could still get "conflicting declarations" with code like this:

```py
try:
    x: int = 1
    x: str = "foo"
except:
    x = "bar"
```

Where (due to exception control flow) we would see both the `x: int` and the `x: str` declarations as "live" at `x = "bar"`. But this seems much less likely in real code.

Another option here is to dis-regard exception control flow entirely when it comes to type declarations. (We would still consider it in type inference, just not for type _declarations_.) So in the previous two examples, when it comes to deciding what the live declarations of `x` are, we would ignore the possibility of jumping directly into `except` from partway through the `try`, and we would consider all declarations in the `try` block as unconditional.

---

_Label `red-knot` added by @carljm on 2024-10-28 16:58_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:24_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-11-18 05:10_

---

_Closed by @dhruvmanila on 2024-12-17 04:19_

---

_Closed by @dhruvmanila on 2024-12-17 04:19_

---
