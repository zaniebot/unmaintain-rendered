```yaml
number: 17184
title: "[syntax-errors] Fix multiple assignment for class keyword argument"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: brent/fix-multiple-class-assignment
created_at: 2025-04-03T19:55:57Z
updated_at: 2025-04-03T21:32:41Z
url: https://github.com/astral-sh/ruff/pull/17184
synced_at: 2026-01-12T15:56:00Z
```

# [syntax-errors] Fix multiple assignment for class keyword argument

---

_@ntBre_

Summary
--

Fixes #17181. The cases being tested with multiple *keys* being equal are actually a slightly different error, more like the error for `MatchMapping` than like the other multiple assignment errors:

```pycon
>>> match x:
...     case Class(x=x, x=x): ...
...
  File "<python-input-249>", line 2
    case Class(x=x, x=x): ...
                      ^
SyntaxError: attribute name repeated in class pattern: x
>>> match x:
...     case {"x": 1, "x": 2}: ...
...
  File "<python-input-251>", line 2
    case {"x": 1, "x": 2}: ...
         ^^^^^^^^^^^^^^^^
SyntaxError: mapping pattern checks duplicate key ('x')
>>> match x:
...     case [x, x]: ...
...
  File "<python-input-252>", line 2
    case [x, x]: ...
             ^
SyntaxError: multiple assignments to name 'x' in pattern
```

This PR just stops the false positive reported in the issue, but I will quickly follow it up with a new rule (or possibly combined with the mapping rule) catching the repeated attributes separately.

Test Plan
--

New inline `test_ok` and updating the `test_err` cases to have duplicate values instead of keys.


---

_Label `bug` added by @ntBre on 2025-04-03 19:55_

---

_Label `preview` added by @ntBre on 2025-04-03 19:55_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-03 19:55_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-03 19:55_

---

_Comment by @github-actions[bot] on 2025-04-03 20:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @dhruvmanila on 2025-04-03 20:35_

Interesting, it seems that class patterns needs to be checked differently:
```pycon
>>> class Foo: ...
... 
... match 1:
...     case Foo(x=1, y={"x": x}):
...         pass
...     case _:
...         pass
...         
>>> 
```
Here, the `x` in class keyword argument is not considered a duplicate from mapping pattern. This makes sense as the `x` in the class pattern is not binding but just trying to match against the class attribute.

---

_@dhruvmanila approved on 2025-04-03 20:37_

---

_Merged by @ntBre on 2025-04-03 21:32_

---

_Closed by @ntBre on 2025-04-03 21:32_

---

_Branch deleted on 2025-04-03 21:32_

---
