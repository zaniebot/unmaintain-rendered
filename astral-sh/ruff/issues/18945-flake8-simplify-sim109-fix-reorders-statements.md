```yaml
number: 18945
title: "[`flake8-simplify`] `SIM109` fix reorders statements incorrectly"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-25T20:49:16Z
updated_at: 2025-08-05T06:40:50Z
url: https://github.com/astral-sh/ruff/issues/18945
synced_at: 2026-01-10T11:09:59Z
```

# [`flake8-simplify`] `SIM109` fix reorders statements incorrectly

---

_Issue opened by @MeGaGiGaGon on 2025-06-25 20:49_

### Summary

I found this while working on #15584

[compare-with-tuple (SIM109)](https://docs.astral.sh/ruff/rules/compare-with-tuple/#compare-with-tuple-sim109) does not respect the `or` ordering in expressions. `x or y == z or y == w` should be turned into `x or (y in (z, w))`, but instead becomes `y in (z, w) or x`, changing the meaning of the code.

https://play.ruff.rs/f7ed8e28-c451-48d8-ace1-c1fdb85f51cd

```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
```
```py
x = None
y = "y"
z = "z"
w = "w"
print(x or y == z or y == w)
```
```
PS ~\Desktop\New_folder\ruff>py issue.py
```
```
False
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select SIM
```
```snap
issue.py:5:7: SIM109 Use `y in (z, w)` instead of multiple equality comparisons
  |
3 | z = "z"
4 | w = "w"
5 | print(x or y == z or y == w)
  |       ^^^^^^^^^^^^^^^^^^^^^ SIM109
  |
  = help: Replace with `y in (z, w)`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select SIM --fix --unsafe-fixes
```
```
Found 1 error (1 fixed, 0 remaining).
```
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
```
```py
x = None
y = "y"
z = "z"
w = "w"
print(y in (z, w) or x)
```
```
PS ~\Desktop\New_folder\ruff>py issue.py
```
```
None
```

Other misc things about `SIM109` I noticed:
The lint currently doesn't apply at all if there are comments inside the expr, but I don't know why. I think it should instead be a normally safe fix, and only unsafe if there are comments. (There is the same amount of `__eq__` calls pre and post fix, ordering in those calls is preserved, and the code already doesn't lint if there is any possibility of other side effects.)

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Renamed from "[`flake8-simplify`] `SIM109` does not respect `or` ordering (false positive)" to "[`flake8-simplify`] `SIM109` fix reorders statements incorrectly" by @MeGaGiGaGon on 2025-06-25 22:19_

---

_Label `bug` added by @ntBre on 2025-06-25 22:24_

---

_Label `fixes` added by @ntBre on 2025-06-25 22:24_

---

_Comment by @ntBre on 2025-06-25 22:30_

Thanks, this seems like a bug. Maybe we can use [`OperatorPrecedence`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/src/operator_precedence.rs#L9) here.

With regard to the fix, I'm not sure if we want to upgrade it to a safe fix when there are no comments, but we could at least avoid the `continue` in the presence of comments since it's already unsafe.

---

_Comment by @mikeleppane on 2025-08-05 06:40_

I'm working on this.

---
