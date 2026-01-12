```yaml
number: 17016
title: "[red-knot] False-positive errors in `finally` suites when all other branches of the `try` block have terminal statements"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
created_at: 2025-03-27T17:10:47Z
updated_at: 2025-05-07T15:22:57Z
url: https://github.com/astral-sh/ruff/issues/17016
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] False-positive errors in `finally` suites when all other branches of the `try` block have terminal statements

---

_@AlexWaygood_

### Summary

Red-knot currently doesn't recognise any symbols in `finally` suites as having visible definitions if the `try` suite has a terminal statement and there either is no `except` suite OR the `except` suite also has terminal statements. This causes quite a few false positives when running red-knot on Black (for example).

```py
def a():
    try:
        return
    except:
        return
    finally:
        f = "foo"
        print(f)  # error: Name `f` is not defined

def b():
    try:
        return
    except:
        return
    finally:
        f = "foo"
        print(f)  # error: Name `f` is not defined


def c():
    try:
        return
    else:
        pass
    finally:
        f = "foo"
        print(f)  # error: Name `f` is not defined


def d():
    try:
        return
    except:
        return
    else:
        pass
    finally:
        f = "foo"
        print(f)  # error: Name `f` is not defined
```

There are no false positives on any of these:

```py
def e():
    try:
        pass
    except:
        return
    finally:
        f = "foo"
        print(f)

def f():
    try:
        return
    except:
        pass
    finally:
        f = "foo"
        print(f)

def g():
    try:
        pass
    except:
        pass
    else:
        return
    finally:
        f = "foo"
        print(f)

def h():
    try:
        return
    except:
        pass
    else:
        return
    finally:
        f = "foo"
        print(f)
```

---

_Label `bug` added by @AlexWaygood on 2025-03-27 17:10_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-27 17:10_

---

_Comment by @carljm on 2025-03-27 19:27_

This is purely a combination of https://github.com/astral-sh/ruff/issues/15797 and https://github.com/astral-sh/ty/issues/233.

A `finally` suite can run either as a result of an exception, or as a result of fall-through from `try` or `except` blocks. We currently model only the latter possibility.

And in the cases shown here, the latter possibility is unreachable, so this triggers our currently-suboptimal handling of unreachable code.

---

_Comment by @AlexWaygood on 2025-03-27 19:42_

Hmm, I see. I suppose this simply validates our decision to treat astral-sh/ruff#15797 as high priority, then :-) I'll close this out in favour of those two issues.

FWIW: this appears to cause about 20 false-positive diagnostics when running red-knot on Black

---

_Closed by @AlexWaygood on 2025-03-27 19:42_

---
