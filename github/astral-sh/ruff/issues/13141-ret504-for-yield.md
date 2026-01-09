---
number: 13141
title: RET504 for yield
type: issue
state: open
author: opk12
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-08-28T15:31:53Z
updated_at: 2024-12-02T17:23:03Z
url: https://github.com/astral-sh/ruff/issues/13141
synced_at: 2026-01-07T13:12:15-06:00
---

# RET504 for yield

---

_Issue opened by @opk12 on 2024-08-28 15:31_

Hi, Python newbie here. `test_return()` warns about unnecessary assignment before return, so I was wondering, what about the same, but for yield?

```
$  cat a.py 
import math


def test_return() -> int:
    """Test `return`."""
    for size in range(1, 10):
        content = math.sqrt(size)
        return content


def test_yield() -> int:
    """Test `yield`."""
    for size in range(1, 10):
        content = math.sqrt(size)
        yield content
```



```
$    ruff check .  --select ALL --isolated
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
a.py:1:1: D100 Missing docstring in public module
a.py:6:5: RET503 [*] Missing explicit `return` at the end of function able to return non-`None` value
a.py:8:16: RET504 [*] Unnecessary assignment to `content` before `return` statement
Found 3 errors.
[*] 2 potentially fixable with the --fix option.
```


```
$  # Debian sid
$  ruff --version
ruff 0.0.291
```

---

_Comment by @JonathanPlasse on 2024-08-28 18:08_

A return statement will exit the function, so the variable of the assignment will never be used.
A yield statement does not exit the function, so the variable of the assignment could be used elsewhere.

---

_Comment by @opk12 on 2024-08-28 19:56_

Good point. For context, I have a custom helper function for non-sequential iteration over Pandas `pd.Series` entries. This helper has no other usage of the variable and has the same core structure as `test_yield()` (which was actually trimmed down from the helper). OTOH this is not a big deal, so I understand if it's not found to be worth the effort.

---

_Comment by @AlexWaygood on 2024-08-29 09:41_

We track the number of references to each binding that's created in our semantic model. We could plausibly emit this lint only if the variable being yielded is a local variable that's only ever used in the `yield` expression. I think this could be a reasonable rule -- though since it doesn't exist in the original `flake8-return` linter, it should probably be a `RUF` rule rather than a `RET` rule.

@JonathanPlasse would you still object to that version of the rule?

---

_Label `rule` added by @AlexWaygood on 2024-08-29 09:42_

---

_Comment by @JonathanPlasse on 2024-08-29 09:52_

I was not objecting to it. I am open to it.

---

_Comment by @AlexWaygood on 2024-08-29 09:57_

Okay, great! Was just checking I hadn't missed something that you'd maybe spotted :D

I'll mark this as "accepted" then

---

_Label `help wanted` added by @AlexWaygood on 2024-08-29 09:57_

---

_Label `accepted` added by @AlexWaygood on 2024-08-29 09:57_

---

_Label `help wanted` removed by @MichaReiser on 2024-12-02 17:23_

---
