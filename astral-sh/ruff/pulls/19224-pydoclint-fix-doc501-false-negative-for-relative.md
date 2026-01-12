```yaml
number: 19224
title: "[`pydoclint`] Fix DOC501 false negative for relative import exceptions"
type: pull_request
state: closed
author: danparizher
labels: []
assignees: []
base: main
head: fix-19219
created_at: 2025-07-09T02:07:17Z
updated_at: 2025-08-20T20:24:07Z
url: https://github.com/astral-sh/ruff/pull/19224
synced_at: 2026-01-12T15:56:34Z
```

# [`pydoclint`] Fix DOC501 false negative for relative import exceptions

---

_@danparizher_

## Summary

Fixes #19219

The problem occurred because the semantic model's `resolve_qualified_name` method would fail completely when trying to resolve relative imports in standalone files where the full module path context is unavailable. The fix adds a fallback mechanism that extracts just the imported member name when full path resolution fails.

---

_Comment by @github-actions[bot] on 2025-07-09 02:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MeGaGiGaGon on 2025-07-09 02:45_

Two things I wonder:
Does this make undefined names trigger the lint, or do they still pass?
What about if the name is a builtin, like would `from . import NotImplementedError` cause the lint to not fire since it would be seen as the same as the builtin, or is there an additional mechanism that distinguishes them?

---

_Comment by @danparizher on 2025-07-09 03:38_

Good points — I tested out both scenarios.

1. I'm making it such that undefined names fail at the semantic analysis stage, so `resolve_qualified_name` returns `None`
2. When you do `from . import NotImplementedError`, you're importing a local module's `NotImplementedError`, not the builtin. My fallback mechanism treats this as a distinct qualified name from `builtins.NotImplementedError`

```py
# Case 1: Builtin (always worked)
def test_builtin():
    """Missing NotImplementedError."""  # DOC501 triggers
    raise NotImplementedError

# Case 2: Relative import (now works) 
from . import NotImplementedError
def test_relative():
    """Missing NotImplementedError."""  # DOC501 now triggers correctly
    raise NotImplementedError
```

---

_Comment by @MeGaGiGaGon on 2025-07-09 04:48_

I think this has introduced a false positive then, since a body of just `raise NotImplementedError` where `NotImplementedError` is the builtin shouldn't raise `DOC501`: https://play.ruff.rs/dd531598-eaa9-4891-bbf9-5d3f745c0b23

From [docstring-missing-exception (DOC501)](https://docs.astral.sh/ruff/rules/docstring-missing-exception/#docstring-missing-exception-doc501):
> This rule is not enforced for abstract methods. It is also ignored for "stub functions": functions where the body only consists of `pass`, `...`, `raise NotImplementedError`, or similar.

And it doesn't look like those cases were ever added to the test file either, so the change didn't show up there either.

---

_Comment by @danparizher on 2025-07-09 21:59_

That makes sense; I messed up the implementation on the first passthrough by modifying the core `resolve_qualified_name`, which broke the stub function detection, so it might make more sense to target the fallback inside DOC501 itself. Would that make sense? Not sure if there are other edge cases or an entirely different approach that would be more effective

---

_Comment by @MeGaGiGaGon on 2025-07-09 22:08_

That was my initial idea, but I'm not sure what the output of `resolve_qualified_name` / all of that part of the semantic model looks like, so I'm not sure.

It also looks like you might have leftover 1 line changes from the old change in `crates/ruff_python_semantic/src/model.rs` / `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP024_0.py.snap`? Unsure on the snap since the test passes, but not sure why your change still affects that one snap.

It also would be a good idea to add at least the `raise NotImplementedError` case as a test.

---

_Comment by @danparizher on 2025-07-09 22:39_

That's true... I'm not too sure why those `pyupgrade` tests would be passing—this is where my knowledge plateaus 😅

---

_Comment by @MichaReiser on 2025-07-11 16:25_

I'm not sure if fixing this for this rule only is the right fix. I think we should look into why the semantic model can't resolve the relative import and see if we can fix that instead. That would benefit all rules instead of just the one rule here.

---

_Comment by @MichaReiser on 2025-08-18 07:37_

@danparizher do you plan to come back to this PR or should we close it for now?

---

_Comment by @danparizher on 2025-08-18 07:40_

@MichaReiser I'll take another look at it this week

---

_Closed by @danparizher on 2025-08-20 20:24_

---
