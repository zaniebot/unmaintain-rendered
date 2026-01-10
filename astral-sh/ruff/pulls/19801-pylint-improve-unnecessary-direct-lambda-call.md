```yaml
number: 19801
title: "[`pylint`] Improve `unnecessary-direct-lambda-call` (`PLC3002`) to ha…"
type: pull_request
state: closed
author: mikeleppane
labels:
  - bug
  - rule
assignees: []
base: main
head: fix(#19135)/PLC3002-causes-false-positive
created_at: 2025-08-07T11:00:20Z
updated_at: 2025-10-12T06:29:39Z
url: https://github.com/astral-sh/ruff/pull/19801
synced_at: 2026-01-10T17:34:34Z
```

# [`pylint`] Improve `unnecessary-direct-lambda-call` (`PLC3002`) to ha…

---

_Pull request opened by @mikeleppane on 2025-08-07 11:00_

## Summary

Improved `unnecessary-direct-lambda-call` ([PLC3002](https://docs.astral.sh/ruff/rules/unnecessary-direct-lambda-call/)) to handle comprehension-related edge cases in class scopes.
The rule now avoids false positives when lambda expressions contain comprehensions that reference class-scoped variables, as inlining these lambdas would cause [F821](https://docs.astral.sh/ruff/rules/undefined-name/) (undefined name) errors. The enhancement adds sophisticated analysis to detect:
- Lambda bodies containing list/set/dict comprehensions or generator expressions
- Class scope contexts where variable access semantics differ after inlining
- Parameter usage within comprehensions that would become undefined after transformation

This prevents unsafe refactoring suggestions while maintaining the rule's effectiveness for genuinely unnecessary lambda calls. 

**Safe cases that now correctly trigger the rule:**
```python
def function():
    # Safe - no comprehensions, triggers PLC3002
    area = (lambda r: 3.14 * r ** 2)(radius)
    
    # Safe - comprehension in function scope, triggers PLC3002
    numbers = [1, 2, 3]
    result = (lambda lst: [x * 2 for x in lst])(numbers)

class A:
    # Safe - comprehension doesn't reference class variables, triggers PLC3002
    y = (lambda: [i for i in range(3)])()
```

**Problematic cases that are now correctly ignored:**
```python
class A:
    x = 1
    # Would cause F821 if inlined: [_ for _ in [1] if x]
    y = (lambda test: [_ for _ in [1] if test])(x)

class Config:
    default_value = 42
    # Would cause F821 if inlined: {k: default_value for k in ['a', 'b']}
    mapping = (lambda val: {k: val for k in ['a', 'b']})(default_value)
```

### PEP709 and Its Impact

**[PEP 709](https://peps.python.org/pep-0709/) (Comprehension inlining)** fundamentally changes Python's scoping semantics for comprehensions, which has direct implications for this fix:

### Current Behavior vs. PEP 709
**Before PEP 709 (Python ≤3.11):**
```python
class A:
    x = 1
    # This would fail with NameError if inlined
    y = (lambda test: [_ for _ in [1] if test])(x)
```
**After PEP 709 (Python 3.12+):**
```python
class A:
    x = 1
    # This now works because comprehensions can access class scope
    y = [_ for _ in [1] if x]  # No longer causes NameError
```

Hopefully, this is a proper approach to add a specialization to the rule to handle problematic related classes and PEP709. In addition, I have taken a conservative approach and will not propose any warnings related to this specific case. 

[ISSUE](https://github.com/astral-sh/ruff/issues/19135)

## Test Plan

```bash
cargo test
```


---

_Comment by @github-actions[bot] on 2025-08-07 11:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-08-08 14:06_

---

_Comment by @Skylion007 on 2025-08-10 17:29_

Instead of discarding the PEP709 case, add support for it, but it gate it behind the minimum Python version config value from ruff. We already have a lot of rules that support syntax in newer versions conditionally based on the specified target minimum or inferred automatically from pyproject.toml

---

_Label `bug` added by @ntBre on 2025-08-27 18:18_

---

_Label `rule` added by @ntBre on 2025-08-27 18:18_

---

_Comment by @ntBre on 2025-08-27 18:30_

> Instead of discarding the PEP709 case, add support for it, but it gate it behind the minimum Python version config value from ruff. We already have a lot of rules that support syntax in newer versions conditionally based on the specified target minimum or inferred automatically from pyproject.toml

I might be missing something, but I don't think there's any version-dependent behavior here. My understanding from the discussion on the issue was that the behavior is the same after PEP 709, but that it's somewhat artificially enforced now instead of being a real function scope (https://github.com/astral-sh/ruff/issues/19135#issuecomment-3036462417). At least that's how I interpreted this comment and a couple of tests on different Python versions.

I think the PR summary here is just incorrect. This example does _not_ work:

```pycon
Python 3.12.9 (main, Feb 12 2025, 14:50:50) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> class A:
...     x = 1
...     # This now works because comprehensions can access class scope
...     y = [_ for _ in [1] if x]  # No longer causes NameError
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 4, in A
NameError: name 'x' is not defined
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_direct_lambda_call.rs`:57 on 2025-08-27 18:33_

I think we should probably fold these two visitors into one. We're currently visiting the `lambda_body` once to check if it contains a comprehension and then again for each parameter in the lambda, which could be really expensive. If we have to have a visitor, we should collect all of the information we need in a single visit.

---

_@ntBre requested changes on 2025-08-27 18:34_

I haven't done a full review, but I think we should combine the separate visitors and separate visitor calls into a single pass, as a start.

---

_Comment by @MichaReiser on 2025-10-12 06:29_

Thanks for the work you put into this. I'll close this PR due to inactivity. A new PR that addresses the feedback is welcomed (by you or any other contributor).

---

_Closed by @MichaReiser on 2025-10-12 06:29_

---
