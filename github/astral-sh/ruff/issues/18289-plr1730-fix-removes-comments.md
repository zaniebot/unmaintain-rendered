---
number: 18289
title: "`PLR1730`: Fix removes comments"
type: issue
state: closed
author: kaddkaka
labels:
  - question
  - fixes
assignees: []
created_at: 2025-05-24T16:30:09Z
updated_at: 2025-05-28T07:51:34Z
url: https://github.com/astral-sh/ruff/issues/18289
synced_at: 2026-01-07T13:12:16-06:00
---

# `PLR1730`: Fix removes comments

---

_Issue opened by @kaddkaka on 2025-05-24 16:30_

### Summary

I just found a code action that loses/removes/drops a comment. I'm worried there are other code actions (or fixes?) that might have similar issue.


With this code and ruleset "PL" enabled:
```py
def a(max_width):
    if max_width < 32:    # <-- this line
        # This comment will be lost
        max_width = 32
```

On the marked line I see these diagnostics:
```
Diagnostics:
1. Ruff: Replace `if` statement with `max_width = max(max_width, 32)` [PLR1730]
2. pylint: [consider-using-max-builtin] Consider using 'max_width = max(max_width, 32)' instead of unnecessary if block [R1731]
```

On the same line I have these actions available:
```
Code actions:
1: Ruff (PLR1730): Replace with `max_width = max(max_width, 32)` [ruff]
2: Ruff (PLR1730): Disable for this line [ruff]
3: Ruff: Fix all auto-fixable problems [ruff]
4: Ruff: Organize imports [ruff]
```

When I perform code action 1, I get this broken result:
```py
def a(max_width):
    max_width = max(max_width, 32)
```
the comment is gone! 😮 

So it looks like when there is a compaction of the code due to an action (or fix?) there is a risk of losing comments.

### Version

0.11.11

---

_Renamed from "code action removes comments" to "`PLR1730: Fix removes comments" by @MichaReiser on 2025-05-25 10:25_

---

_Comment by @MichaReiser on 2025-05-25 10:25_

Playground: https://play.ruff.rs/895cfa46-7386-4810-8f07-d6db8be8b9bc

---

_Renamed from "`PLR1730: Fix removes comments" to "`PLR1730`: Fix removes comments" by @MichaReiser on 2025-05-25 10:25_

---

_Comment by @MichaReiser on 2025-05-25 10:42_

Thanks for reporting this error. This should have been fixed by https://github.com/astral-sh/ruff/pull/17459, so that the fix is now marked as unsafe 
> Specifically, an unsafe fix could lead to a change in runtime behavior, the removal of comments, or both, while safe fixes are intended to preserve runtime behavior and will only remove comments when deleting entire statements or expressions (e.g., removing unused imports).

This means the fix shouldn't be applied when running ruff from the CLI unless you opt in to unsafe fixes

```
uvx ruff@latest check lib.py --no-cache --select PLR --fix
stalled 1 package in 2ms
lib.py:2:5: PLR1730 Replace `if` statement with `max_width = max(max_width, 32)`
  |
1 |   def a(max_width):
2 | /     if max_width < 32:    # <-- this line
3 | |         # This comment will be lost
4 | |         max_width = 32
  | |______________________^ PLR1730
  |
  = help: Replace with `max_width = max(max_width, 32)`

lib.py:2:20: PLR2004 Magic value used in comparison, consider replacing `32` with a constant variable
  |
1 | def a(max_width):
2 |     if max_width < 32:    # <-- this line
  |                    ^^ PLR2004
3 |         # This comment will be lost
4 |         max_width = 32
  |

Found 2 errors.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

But it still gets shown in the IDE because we still think it's useful and a user would notice that the comment gets removed, and can decide if that's okay or if they undo the actio nand rather do the refactor manually

---

_Label `question` added by @MichaReiser on 2025-05-25 10:42_

---

_Label `fixes` added by @MichaReiser on 2025-05-25 10:42_

---

_Comment by @kaddkaka on 2025-05-25 11:01_

Aha, I see. I was surprised, and leaving the comment above or on the line would have surprised me less. Would it be possible and an improvement to change the fix so that the result is:

alt 1.
```py
def a(max_width):
    max_width = max(max_width, 32)  # This comment will be lost
```

or alt2.

```py
def a(max_width):
    # This comment will be lost
    max_width = max(max_width, 32)
```
?

-----

The explanation of the rule mentions "fix is sometimes available", but doesn't mention if it's safe or not. 

1. I wonder, is it worthwhile to elaborate on this explanation?
2. Also would it be possible to detect whether the transformation can de be done safely or not? Maybe that's not how fixes are categorized/behave.

`````console
$ ruff rule PLR1730
# if-stmt-min-max (PLR1730)

Derived from the **Pylint** linter.

Fix is sometimes available.

## What it does
Checks for `if` statements that can be replaced with `min()` or `max()`
calls.

## Why is this bad?
An `if` statement that selects the lesser or greater of two sub-expressions
can be replaced with a `min()` or `max()` call respectively. Where possible,
prefer `min()` and `max()`, as they're more concise and readable than the
equivalent `if` statements.

## Example
```python
if score > highest_score:
    highest_score = score
```

Use instead:
```python
highest_score = max(highest_score, score)
```

## References
- [Python documentation: max function](https://docs.python.org/3/library/functions.html#max)
- [Python documentation: min function](https://docs.python.org/3/library/functions.html#min)
`````

---

_Comment by @MichaReiser on 2025-05-25 11:05_

> Aha, I see. I was surprised, and leaving the comment above or on the line would have surprised me less. Would it be possible and an improvement to change the fix so that the result is:

It's possible but a lot of work. Some fixes do just that but not all. For now, we opted to mark fixes that don't always preserve comments as always.

> I wonder, is it worthwhile to elaborate on this explanation?

Yes, we should. We're working on it, see https://github.com/astral-sh/ruff/issues/15584

> Also would it be possible to detect whether the transformation can de be done safely or not? Maybe that's not how fixes are categorized/behave.

This is already done. We only mark the fix as unsafe if it may remove comments. It is always marked as safe if there are no comments in the range.

---

_Closed by @MichaReiser on 2025-05-28 07:51_

---
