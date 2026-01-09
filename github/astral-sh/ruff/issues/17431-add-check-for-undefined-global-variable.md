---
number: 17431
title: Add check for undefined global variable
type: issue
state: open
author: nickdrozd
labels:
  - rule
assignees: []
created_at: 2025-04-16T18:05:06Z
updated_at: 2025-04-16T18:35:36Z
url: https://github.com/astral-sh/ruff/issues/17431
synced_at: 2026-01-07T13:12:16-06:00
---

# Add check for undefined global variable

---

_Issue opened by @nickdrozd on 2025-04-16 18:05_

### Summary

Python has some strange behavior with `global`. Consider:

```python
def f() -> None:
    global x
    x = 5

f()

print(x)
```

This looks like it should raise an error, but it runs fine.

Ruff doesn't flag this behavior, but I think it should, because it is probably indicative of an error. For example, say I am renaming a global variable:

```python
X = 4  # change name from x to X

def f() -> None:
    global x
    x = 5
```

Now `f` refers to `x` var while actual global var `X` is unaffected. Very bad!

Pylint flags this as `global-variable-undefined`. Mypy objects too. Ruff says nothing, so it cannot be relied upon for global var refactoring.

---

_Comment by @ntBre on 2025-04-16 18:35_

I think I actually just ran into the cause of this yesterday in #17411. We currently add a binding to the global scope if we encounter a `global` statement before the variable has actually been defined. This makes us consistent with Python in weird cases like your first example (and one of our [`F821` test cases](https://github.com/astral-sh/ruff/blob/1a79722ee0fb160f8929612508d5ee88b7838d09/crates/ruff_linter/resources/test/fixtures/pyflakes/F821_6.py) ), but I think it will also make it difficult to detect this case without some changes to our semantic model.

---

_Label `rule` added by @ntBre on 2025-04-16 18:35_

---
