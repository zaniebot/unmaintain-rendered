---
number: 12911
title: "F601 `multi-value-repeated-key-literal` various issues"
type: issue
state: closed
author: dylwil3
labels:
  - needs-design
assignees: []
created_at: 2024-08-15T21:39:18Z
updated_at: 2024-08-16T06:49:26Z
url: https://github.com/astral-sh/ruff/issues/12911
synced_at: 2026-01-07T13:12:15-06:00
---

# F601 `multi-value-repeated-key-literal` various issues

---

_Issue opened by @dylwil3 on 2024-08-15 21:39_

This is an extension of #12772.

# Observed issues
[Playground Link](https://play.ruff.rs/e6b35fd7-dca9-421a-915a-fa3d34f04dd9)

- F601 should only be triggered for Expr's whose leaves are literals of some kind - the following two examples ought to fall under F602 instead.

```python
{(foo, bar): "a", (foo, bar): "b"}

{
    f"{foo}": "a",
    f"{foo}": "b",
}
```

- As in https://github.com/astral-sh/ruff/issues/12772 booleans and numbers that are equal should trigger this rule.

```python
{
    True: "a",
    1: "b",
    1.0: "c",
    1.0
    + 0.0j: "d",  # <-- this may be out of scope depending on the implementation of the fix
    2: "e",
    2.0: "f",
}
```

- (Maybe a feature request rather than a bug) It would be nice to extend F601 to situations like this, where expression ASTs have _literal_ leaves and internal nodes do not involve a function call

```python
{
    -1: "a",
    -1: "b",
}

{
    1 + 1: "a",
    1 + 1: "b",
}

# but _not_ to situations which require any kind of actual evaluation
{
    1 + 1: "a",
    2: "b",
}

# except maybe this, which appears above as well
# and which is arguably a casting/type promotion
{
    1: "a",
    1.0 + 0.0j: "b",
}
```

- There is also an issue with the suggested fix for values which are the "same", as there could be side effects, eg

```python
# Suggested fix is unsafe, since we can't know
# which value of iterator was desired
{
    1: next(it),
    1: next(it),
}
```

- Finally, a minor issue: the name of the rule does not seem quite right, since the linter complains if a key is repeated even if the same value appears.

# Potential Solution

I propose that the _logic_ of this check (independently of the implementation) should be as follows:

1. F601 should ignore keys which do not satisfy all of the following:
    - They are expressions that have only literals as leaves 
    - They are expressions where no node is a `Name` (that may be redundant since Name's are terminal?)
2. F601 should consider keys duplicated if the corresponding `ComparableExpr`'s are equal after further replacing all boolean and numeric literals with their complex-valued equivalent (for these purposes, I think "large" integers should be left as is).
3. F601 should not propose a fix if the "unique" value found has possible side effects - for example, we could just never propose a fix if the value found involves a function call.
4. Change the name of the rule to `repeated-key-literal` (and similarly for F602).


---

_Comment by @dylwil3 on 2024-08-15 21:47_

By the way, I'm happy to work on this I just wanna pin down the "assignment" first.

---

_Label `needs-design` added by @MichaReiser on 2024-08-16 06:47_

---

_Comment by @MichaReiser on 2024-08-16 06:49_

I hope you don't mind if I convert this to a discussion.

---

_Locked by @MichaReiser on 2024-08-16 06:49_

---
