```yaml
number: 21191
title: "FURB142: Parenthesize generator expression arguments in fixer to preserve scoping"
type: pull_request
state: open
author: sohamukute
labels:
  - bug
  - fixes
assignees: []
base: main
head: FURB142
created_at: 2025-11-01T20:12:15Z
updated_at: 2025-11-10T12:10:51Z
url: https://github.com/astral-sh/ruff/pull/21191
synced_at: 2026-01-10T16:53:55Z
```

# FURB142: Parenthesize generator expression arguments in fixer to preserve scoping

---

_Pull request opened by @sohamukute on 2025-11-01 20:12_

## Summary

Fixes a bug in the FURB142 (for-loop-set-mutations) fixer where an unparenthesized generator expression used as the `.add(...)` argument would be inlined into the rewritten comprehension without parentheses, changing variable scoping and causing a runtime NameError.

Before (problematic after fix):
```python
s = set()
for x in ("abc", "def"):
    s.add(c for c in x)
# fixed to:
s.update(c for c in x for x in ("abc", "def"))  # NameError: x
```

After (correct, parenthesized):
```python
s = set()
for x in ("abc", "def"):
    s.add(c for c in x)
# fixed to:
s.update((c for c in x) for x in ("abc", "def"))
```

Implementation details:
- In the “comprehension” rewrite path of FURB142, detect `Expr::Generator` arguments that are not already parenthesized and wrap them with parentheses in the generated replacement string.
- Leaves already-parenthesized generator expressions unchanged.
- Leaves non-generator arguments unchanged.
- Maintains existing applicability handling and comment-safety behavior.

References:
- Fixes #21098
- Rule docs: FURB142 (for-loop-set-mutations)

## Test Plan

- Repro the original failing case and confirm the new fix:

  1) Create the file:
     ```python
     s = set()
     for x in ("abc", "def"):
         s.add(c for c in x)
     ```

  2) Verify Ruff flags it:
     ```
     cargo run -p ruff_cli -- check --isolated --select FURB142 --preview furb142.py
     ```

  3) Apply the fix:
     ```
     cargo run -p ruff_cli -- check --isolated --select FURB142 --preview --fix furb142.py
     ```

  4) Inspect output (should contain parentheses):
     ```
     s.update((c for c in x) for x in ("abc", "def"))
     ```

  5) Run the file:
     ```
     python furb142.py
     ```
     Expect no NameError.


---

_Comment by @ntBre on 2025-11-06 15:47_

Thanks! Could you add some [snapshot tests](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#rule-testing-fixtures-and-snapshots) too? Basically just including your manual tests in the repo to avoid future regressions.

The code looks good to me otherwise.

---

_Label `bug` added by @ntBre on 2025-11-06 15:47_

---

_Label `fixes` added by @ntBre on 2025-11-06 15:47_

---

_Comment by @github-actions[bot] on 2025-11-06 15:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sohamukute on 2025-11-07 19:48_

> Thanks! Could you add some [snapshot tests](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#rule-testing-fixtures-and-snapshots) too? Basically just including your manual tests in the repo to avoid future regressions.
> 
> The code looks good to me otherwise.

okay will add them.

---

_@MichaReiser requested changes on 2025-11-10 12:10_

Requesting changes to make it clear that this PR has unaddressed feedback and isn't waiting for a review. 

Once you've updated the PR, please re-request review.

---
