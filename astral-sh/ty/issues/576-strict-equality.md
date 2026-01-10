```yaml
number: 576
title: strict-equality
type: issue
state: open
author: zwing99
labels:
  - lint
assignees: []
created_at: 2025-06-03T14:33:03Z
updated_at: 2025-12-16T16:29:43Z
url: https://github.com/astral-sh/ty/issues/576
synced_at: 2026-01-10T01:54:59Z
```

# strict-equality

---

_Issue opened by @zwing99 on 2025-06-03 14:33_

### Question

Is tests for strict equality on the roadmap for ty. In mypy we can check for "strict equality" using the `--strict-equality` flag or equivalent configuration setting. This check flags a class of error where the developer might accidentally be comparing disparate types for equality. Naively it looks like this but you can imagine more complex scenarios.

Example:
```python
an_int: int = 42
a_str: str = "42"

if an_int == a_str:
    print("This should not print, as int and str are not equal")
```

mypy returns:
```
main.py:###: error: Non-overlapping equality check (left operand type: "int", right operand type: "str")  [comparison-overlap]
```

ty does not warn or give an error and all the default ignored rules do not seem like they would capture this.

Anyways, the point is me wondering if this is on ty's roadmap? It is a feature of mypy that i discovered recently and seems very valuable!

### Version

0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Label `question` added by @zwing99 on 2025-06-03 14:33_

---

_Comment by @carljm on 2025-06-03 16:18_

I think this is a rule that would fit better into a type-aware linter that we intend to eventually build on top of ty. It's not a type error in Python to compare an integer and a string. (In fact, if all we know is the types `int` and `str`, it's not even a definitely-false comparison: the types `int` and `str` include user subclasses of `int` and `str`, which can override `__eq__` to compare equal to anything.) So for these reasons we'd be unlikely to add a rule like this as a core type-checker diagnostic. But it does seem like it could be practically useful as a more opinionated lint rule.

---

_Comment by @zwing99 on 2025-06-03 20:20_

Agree it more likely a opt-in feature than an on by default feature for all the reasons you bring up.  If it is a feature on top of ty or part of ty is fine by me. Just a useful lint to trap certain kinds of bugs.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:33_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Label `question` removed by @MichaReiser on 2025-11-24 08:20_

---

_Label `lint` added by @MichaReiser on 2025-11-24 08:20_

---

_Comment by @AlexWaygood on 2025-12-16 12:34_

> I think this is a rule that would fit better into a type-aware linter that we intend to eventually build on top of ty. It's not a type error in Python to compare an integer and a string. (In fact, if all we know is the types `int` and `str`, it's not even a definitely-false comparison: the types `int` and `str` include user subclasses of `int` and `str`, which can override `__eq__` to compare equal to anything.) So for these reasons we'd be unlikely to add a rule like this as a core type-checker diagnostic. But it does seem like it could be practically useful as a more opinionated lint rule.

While this is all true, I've found mypy's `strict-equality` error code very useful on my Python projects. It's very effective at finding bugs and rarely has false positives. If we want feature parity with existing type checkers, I think we should consider putting this on the roadmap for our stable release.

---

_Added to milestone `Stable` by @carljm on 2025-12-16 16:29_

---
