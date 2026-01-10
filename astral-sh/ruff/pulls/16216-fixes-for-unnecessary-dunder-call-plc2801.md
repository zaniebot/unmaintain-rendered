```yaml
number: 16216
title: Fixes for unnecessary-dunder-call (PLC2801)
type: pull_request
state: open
author: VascoSch92
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix-plc2801
created_at: 2025-02-17T16:02:15Z
updated_at: 2025-03-09T14:17:10Z
url: https://github.com/astral-sh/ruff/pull/16216
synced_at: 2026-01-10T19:49:01Z
```

# Fixes for unnecessary-dunder-call (PLC2801)

---

_Pull request opened by @VascoSch92 on 2025-02-17 16:02_

The PR addresses issue #16053 

The bugs addressed are:
- `print((not 1).__add__(1))`
- `try: print(list("x".__add__(y for y in "y"))) except TypeError as e: print(e)`
- `print(("a" and "x").__contains__("x"))`
- `print(("" or "x").__contains__("x"))`
- `print(("" if False else "x").__contains__("x"))`
- `print((x := "x").__contains__("y"))`
- `print((not 0).__radd__(1))`
- `print(3 - (2).__add__(1))`
- `print("x".__add__(*"y"))`

@MichaReiser I don't think I have the best implementation. Perhaps, you have some hints how to make the code better ;-)


---

_Comment by @github-actions[bot] on 2025-02-17 16:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-02-17 19:48_

---

_Label `fixes` added by @ntBre on 2025-02-17 19:48_

---

_Comment by @MichaReiser on 2025-02-18 08:28_

@VascoSch92 can you point towards the code that you think isn't ideal or can you explain me what you're trying to do?

---

_Comment by @VascoSch92 on 2025-02-18 10:14_

@MichaReiser Yes my bad.

The logic behind to find a fix for a dunder call of the form `a.__dunder__(b)` is the following:

1. Understand who has the precedenze between `a` and `b`, i.e., fix of the form `a dunder_operation b` or `b dunder_operation a`.
2. Understand if one or more of `a`, `b` or the whole `expression` has to be wrapped in parentheses, i.e., one of the following cases:

    - `(a) dunder_operation b`
    - `a dunder_operation (b)`
    - `(a dunder_operation b)`
    - `(a dunder_operation (b))`
    - and so on...

Therefore, 
- I added a method called needs_parentheses (line 263), which determines whether an and b require parentheses. To achieve this, I simply check whether a or b contains a keyword. However, the method feels somewhat hardcoded, and there may be a more efficient way to determine if parentheses are needed.
- Note that I did not implement all possible ramifications of the logic described above. I only implemented the ones necessary to address the bugs reported in the related issue.

---

_Comment by @VascoSch92 on 2025-03-07 11:10_

@MichaReiser gently reminder :-) 

---

_Comment by @MichaReiser on 2025-03-08 18:16_

Sorry for the long wait. I'm not sure when I'll get to this. I have to play with the code myself because the solution isn't obvious to me.

---

_Comment by @VascoSch92 on 2025-03-09 14:17_

> Sorry for the long wait. I'm not sure when I'll get to this. I have to play with the code myself because the solution isn't obvious to me.

Don't worry :) perhaps i also should spend sometime to make the code more clear...

---
