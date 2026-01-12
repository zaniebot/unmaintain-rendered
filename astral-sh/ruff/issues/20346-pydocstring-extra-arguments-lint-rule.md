```yaml
number: 20346
title: pydocstring extra-arguments lint rule
type: issue
state: closed
author: blackmad-cradle
labels: []
assignees: []
created_at: 2025-09-11T11:17:54Z
updated_at: 2025-09-11T13:01:42Z
url: https://github.com/astral-sh/ruff/issues/20346
synced_at: 2026-01-12T15:54:57Z
```

# pydocstring extra-arguments lint rule

---

_@blackmad-cradle_

### Summary

I noticed when adding in pydocstring lint rules that ruff doesn't have a rule to warn when there's an argument definition in a docstring that isn't in the function parameters. This would be good to have when refactoring code.

Here's a minimal repro with the pydocstring rules turned on: https://play.ruff.rs/0f9db152-4bdf-4c4f-a4e9-8b3ccba1848c

I couldn't find another issue tracking this, so I'm starting a new one. If this is a dupe, apologies.

`"""example."""
def fibonacci(n):
    """Compute the nth number in the Fibonacci sequence.
    
    Args:
    n: number
    x: extra arg
    """
    pass`

I would like/expect to see an error for "x: extra arg"

---

_Comment by @ntBre on 2025-09-11 13:01_

Interesting, I assumed we had a rule for that too. The closest, but opposite, is [undocumented-param (D417)](https://docs.astral.sh/ruff/rules/undocumented-param/#undocumented-param-d417). I think this case would be covered by `DOC102 Docstring contains more arguments than in function signature` from https://github.com/astral-sh/ruff/issues/12434, though. I see that there's a PR for DOC102 actually, I'll comment there as well.

---

_Closed by @ntBre on 2025-09-11 13:01_

---
