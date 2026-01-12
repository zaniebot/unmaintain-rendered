```yaml
number: 21069
title: "[`ruff`] Suppress strings with backslashes in interpolations before Python 3.12 (`RUF027`)"
type: pull_request
state: open
author: danparizher
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix-21033
created_at: 2025-10-24T22:54:21Z
updated_at: 2025-10-27T22:25:32Z
url: https://github.com/astral-sh/ruff/pull/21069
synced_at: 2026-01-12T15:57:15Z
```

# [`ruff`] Suppress strings with backslashes in interpolations before Python 3.12 (`RUF027`)

---

_@danparizher_

## Summary
Fixes #21033 (RUF027) to suppress f-string suggestions for strings containing backslashes in interpolation expressions when targeting Python versions below 3.12, preventing syntax errors.

## Problem Analysis
RUF027 was suggesting f-string conversions for strings with backslashes in interpolation expressions (e.g., `"{'\\n'}"`), but f-strings with backslashes in interpolations are only valid syntax in Python 3.12+. This caused syntax errors when the fix was applied to code targeting older Python versions.

The issue occurred because the `should_be_fstring` function was checking for backslashes anywhere in the string literal, rather than specifically within the interpolation expressions where the syntax restriction applies.

## Approach
Added a version check in the `should_be_fstring` function to suppress RUF027 for strings containing backslashes within interpolation expressions when the target Python version is below 3.12. This ensures:

- For Python < 3.12: RUF027 is suppressed for strings with backslashes in interpolations (e.g., `"{'\\n'}{x}"`)
- For Python ≥ 3.12: RUF027 works normally for strings with backslashes in interpolations


---

_Comment by @github-actions[bot] on 2025-10-24 23:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dscorbett on 2025-10-25 15:02_

Backslashes in f-strings aren’t the problem.
```pycon
>>> f"\n"
'\n'
```
The problem is backslashes in f-string interpolations.
```pycon
>>> f"{'\n'}"
  File "<stdin>", line 1
    f"{'\n'}"
             ^
SyntaxError: f-string expression part cannot include a backslash
```

---

_Renamed from "[`ruff`] Suppress strings with backslashes before Python 3.12 (`RUF027`)" to "[`ruff`] Suppress strings with backslashes in interpolations before Python 3.12 (`RUF027`)" by @danparizher on 2025-10-25 21:26_

---

_Comment by @ntBre on 2025-10-27 18:19_

I just worked on two issues for our syntax error related to this: https://github.com/astral-sh/ruff/pull/20867 and https://github.com/astral-sh/ruff/pull/20949. I think the fix here is similar to the incorrect code I had originally in the parser, which turned out to be too naive in the presence of nested interpolations. To summarize the two issues, code like this is valid on 3.11:

```pycon
>>> f"{1:\"d\"}"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Invalid format specifier '"d"' for object of type 'int'
```

Note the `ValueError`, not an f-string syntax error. It looks like the code here would flag this because it's within the interpolation but not within the interpolation _expression_.

In contrast, code like this is still invalid on 3.11 because it's in the expression part of a nested interpolation:

```pycon
>>> f'{1: abcd "{'aa'}" }'
  File "<stdin>", line 1
    f'{1: abcd "{'aa'}" }'
                  ^^
SyntaxError: f-string: expecting '}'
```

But the `interpolations` iterator doesn't recurse into nested interpolations, so the fix is also not as easy as only checking the expression part (that's what I got wrong in #20867).

It might be fair to be more conservative here since it's almost definitely easier to implement, but I wanted to mention this since it was relevant.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:183 on 2025-10-27 18:20_

I think we could drop the `locator` and `semantic` arguments if we're passing the `Checker` they both come from. Alternatively, we could pass only the `target_version`, which is the only additional field we're accessing now.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF027_RUF027_0.py.snap`:1 on 2025-10-27 18:20_

We should add a variant of this test where we use a Python version before 3.12. This is still showing a diagnostic being emitted.

---

_@ntBre reviewed on 2025-10-27 18:21_

---

_Label `bug` added by @ntBre on 2025-10-27 18:21_

---

_Label `fixes` added by @ntBre on 2025-10-27 18:21_

---

_Review requested from @ntBre by @danparizher on 2025-10-27 22:15_

---
