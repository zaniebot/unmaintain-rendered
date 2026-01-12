```yaml
number: 20203
title: superfluous-literal-comparison
type: issue
state: open
author: dorschw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-02T08:10:56Z
updated_at: 2025-09-12T23:51:42Z
url: https://github.com/astral-sh/ruff/issues/20203
synced_at: 2026-01-12T15:54:57Z
```

# superfluous-literal-comparison

---

_@dorschw_

### Summary

Code like this is likely unintended and redundant:
```python
if x == 0 and x != 1: ...
```
To keep things simple, only check comparison with literals, not with variables (allow `x == variable and x != other_variable`)

---

_Label `rule` added by @ntBre on 2025-09-02 12:52_

---

_Label `needs-decision` added by @ntBre on 2025-09-02 12:52_

---

_Comment by @MeGaGiGaGon on 2025-09-12 23:51_

I'm not sure how well this would fit in ruff (a linter), since it could also be the job of type checkers with `int` -> `Literal` narrowing. 

Pyright supports this with strict mode [playground](https://pyright-play.net/?strict=true&code=B4AgvCCWB2AuAUMAOBXBBKdBYAUJAZiKGBAAwgCG0AJkSAIQQCMAXCAHSdA)

Mypy doesn't fully support the example yet, but it is being considered https://github.com/python/mypy/issues/16819 . Code with the variable already narrowed to literal and strict enabled also gives an error on similar code [playground](https://mypy-play.net/?mypy=latest&python=3.12&flags=strict&gist=64a94142b6863f717d4a66d79b8068b0)

---

If it would apply to arbitrary code, there's also the issues of mutating calls in the code, ie
```py
if next(x) == 0 and next(x) != 1:...
```
So it should be restricted to just working on simple variables and literals.

Some additional considerations, would this also support strings?
```py
if x == "a" and x != "b":...
```
What about membership tests on collections comprised of simple literals?
```py
if x in [1, 2, 3] and x != 4:...
```

---
