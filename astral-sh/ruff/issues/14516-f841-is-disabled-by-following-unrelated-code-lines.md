```yaml
number: 14516
title: F841 is disabled by following, unrelated code lines
type: issue
state: closed
author: kud1ing
labels: []
assignees: []
created_at: 2024-11-21T13:30:05Z
updated_at: 2024-11-21T13:53:04Z
url: https://github.com/astral-sh/ruff/issues/14516
synced_at: 2026-01-10T11:09:56Z
```

# F841 is disabled by following, unrelated code lines

---

_Issue opened by @kud1ing on 2024-11-21 13:30_

This is with ruff 0.7.4.

`a` is recognized correctly as unused here (`F841 Local variable 'a' is assigned to but never used`):
```
def foo(x):
    if x:
        a = "hello"
        return
```
But if you add lines below, ruff stops recognizing it as unused (`All checks passed!`):
```
def foo(x):
    if x:
        a = "hello"
        return

    a = "goodbye"
    print(a)
```


---

_Comment by @MichaReiser on 2024-11-21 13:53_

Ruff's "is used" detection isn't branch sensitive. This means, Ruff isn't aware that `a = "hello"` is only specific to the `if x:` branch.  All it sees is that there's a definition of `a` and that it is used somewhere inside `foo`. Thus, the variable is marked as unused. 

This is similar to https://github.com/astral-sh/ruff/issues/14355 and other linked issues. We're working on a new analysis engine that allows us to dedect unused assignments in individual branches, but it will be a while before we can use that in F841.


---

_Closed by @MichaReiser on 2024-11-21 13:53_

---
