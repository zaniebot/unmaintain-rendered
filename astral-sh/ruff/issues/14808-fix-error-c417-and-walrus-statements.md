---
number: 14808
title: "[Fix error] C417 and walrus statements"
type: issue
state: closed
author: zeevox
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2024-12-06T08:14:26Z
updated_at: 2024-12-07T03:00:35Z
url: https://github.com/astral-sh/ruff/issues/14808
synced_at: 2026-01-10T01:22:55Z
---

# [Fix error] C417 and walrus statements

---

_Issue opened by @zeevox on 2024-12-06 08:14_

Applying fixes for rule [C417](https://docs.astral.sh/ruff/rules/unnecessary-map/) introduces a syntax error when the second `map` argument is a walrus assignment operator. 

Minimal example below.
I'm using `ruff` 0.8.2 and Python 3.12.7 to test this.
For a more complete example, this issue first arose in my [solution](https://github.com/zeevox/advent-of-code/blob/26a86375aab513f4020b6264d7096561d031ba03/2019/python/Day03.py#L24-L27) to Advent of Code [2019 Day 3](https://adventofcode.com/2019/day/3). 

```diff
a = [1, 2, 3]
b = map(lambda x: x, c := a)
print(c)
```

Fix applied by `ruff check test.py --select C417 --fix --unsafe-fixes`

```diff
-b = map(lambda x: x, c := a)
+b = (x for x in c := a)
```

is syntactically incorrect

```python
  File "/home/zeevox/test.py", line 2
    b = (x for x in c := a)
                      ^^
SyntaxError: invalid syntax
```

You would think wrapping the walrus statement in parentheses would suffice, like

```python
b = (x for x in (c := a))
```

And indeed, `ruff` believes all is well

```
% ruff check test.py --select C417
All checks passed!
```

but this too, in fact, raises a syntax error

```python
  File "/home/zeevox/test.py", line 2
    b = (x for x in (c := a))
                     ^^^^^^
SyntaxError: assignment expression cannot be used in a comprehension iterable expression
```

I wonder whether the real solution is to extract the assignment onto the previous line, or not show the warning in the first place.

---

_Label `bug` added by @AlexWaygood on 2024-12-06 08:36_

---

_Label `fixes` added by @AlexWaygood on 2024-12-06 08:36_

---

_Label `help wanted` added by @AlexWaygood on 2024-12-06 08:36_

---

_Comment by @harupy on 2024-12-06 11:41_

@AlexWaygood Can this change fix the error?

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs b/crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs
index 4761c80b8..9e2baec85 100644
--- a/crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs
+++ b/crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs
@@ -697,7 +697,7 @@ pub(crate) fn fix_unnecessary_map(
     let iter = iter.clone();
     let iter = if iter.lpar().is_empty()
         && iter.rpar().is_empty()
-        && matches!(iter, Expression::IfExp(_) | Expression::Lambda(_))
+        && matches!(iter, Expression::IfExp(_) | Expression::Lambda(_) | Expression::NamedExpr(_))
     {
         iter.with_parens(LeftParen::default(), RightParen::default())
     } else {
```

Never mind. Didn't read the issue carefully.

---

_Referenced in [astral-sh/ruff#14827](../../astral-sh/ruff/pulls/14827.md) on 2024-12-06 23:21_

---

_Closed by @charliermarsh on 2024-12-07 03:00_

---
