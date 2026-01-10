```yaml
number: 1792
title: "SIM108 fixer: enhance refactoring for shared function call"
type: issue
state: closed
author: spaceone
labels:
  - rule
assignees: []
created_at: 2023-01-11T22:41:38Z
updated_at: 2023-06-08T16:59:48Z
url: https://github.com/astral-sh/ruff/issues/1792
synced_at: 2026-01-10T11:09:44Z
```

# SIM108 fixer: enhance refactoring for shared function call

---

_Issue opened by @spaceone on 2023-01-11 22:41_

```
$ ruff --isolated  --select SIM108 foo.py
foo.py:1:1: SIM108 Use ternary operator `foo = response(1) if item is None else response(item)` instead of if-else-block
Found 1 error(s).
1 potentially fixable with the --fix option.
$ cat foo.py
if item is None:
   foo = response(1)
else:
   foo = response(item)
$ ruff --isolated  --select SIM108 --fix foo.py
Found 1 error(s) (1 fixed, 0 remaining).
$ cat foo.py
foo = response(1) if item is None else response(item)
```

A nicer result would be:
`foo = response(1 if item is None else item)`

---

_Comment by @charliermarsh on 2023-01-11 23:39_

I think this would need to be an entirely separate lint rule.

---

_Label `rule` added by @charliermarsh on 2023-01-11 23:48_

---

_Comment by @charliermarsh on 2023-01-11 23:49_

The rule being: if you have a ternary, and it's just calling a function with different arguments, we should move the ternary into the function (hopefully can expressed more generally).

---

_Comment by @charliermarsh on 2023-06-08 16:59_

This is specific enough (same call, single argument) that I'm not going to prioritize it.

---

_Closed by @charliermarsh on 2023-06-08 16:59_

---
