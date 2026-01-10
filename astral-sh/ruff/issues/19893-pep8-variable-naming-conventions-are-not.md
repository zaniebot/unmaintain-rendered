---
number: 19893
title: PEP8 variable naming conventions are not correctly enforced
type: issue
state: open
author: t-m-sharpe
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-13T11:47:49Z
updated_at: 2025-08-13T13:22:29Z
url: https://github.com/astral-sh/ruff/issues/19893
synced_at: 2026-01-10T01:23:01Z
---

# PEP8 variable naming conventions are not correctly enforced

---

_Issue opened by @t-m-sharpe on 2025-08-13 11:47_

### Summary

PEP8 quite clearly states [here](https://peps.python.org/pep-0008/#function-and-variable-names) that:

> Function names should be lowercase, with words separated by underscores as necessary to improve readability.
> 
> Variable names follow the same convention as function names.

It also states that these same conventions should be followed for [global variables](https://peps.python.org/pep-0008/#global-variable-names) and [instance variables](https://peps.python.org/pep-0008/#method-names-and-instance-variables). In other words, variable names should be `snake_case` in all contexts.

However, even with `ALL` rules enabled, Ruff does not complain about the use of `CapWords` in global variable names, or `CapWords` or `mixedCase` in instance variable names. See [this playground](https://play.ruff.rs/c45e5b1d-ad7f-4c60-b731-b4ddc9fa838d) for a demonstration of this. 

**Ruff should have a rule to enforce variable names to be in `snake_case` in all contexts.**

---

As an aside, I believe the relevant section of PEP8 may have been misunderstood only to enforce `snake_case` for variables defined inside function scopes, hence rule [N806](https://docs.astral.sh/ruff/rules/non-lowercase-variable-in-function/). As noted in [this StackOverflow post](https://stackoverflow.com/questions/71340765/logic-behind-why-python-variables-should-be-lowercase-in-functions-but-outside-f), PEP8 in fact makes no distinction between variables defined inside function scopes or elsewhere.

### Version

ruff 0.12.2 (9bee8376a 2025-07-03)
Also ruff 0.12.8 in the playground.

---

_Comment by @ntBre on 2025-08-13 12:56_

Thanks for the report! This seems to be consistent with the upstream [pep8-naming](https://github.com/PyCQA/pep8-naming), which also doesn't flag anything in the playground example, but I didn't find any reasoning there for why the global variables and instance attributes are excluded.

I think the global exclusion _might_ be to avoid false positives if it happens to be a class. Changing `ThisShouldBeFlagged` to `thisShouldBeFlagged` does get flagged by N816. Not sure about the attributes, though.

---

_Label `rule` added by @ntBre on 2025-08-13 12:56_

---

_Label `needs-decision` added by @ntBre on 2025-08-13 12:56_

---

_Comment by @MichaReiser on 2025-08-13 12:59_

There's at least https://github.com/astral-sh/ruff/issues/2964 and I think there are some more existing issues that cover the same or at least a similar issue.

---

_Comment by @t-m-sharpe on 2025-08-13 13:04_

#2964 only seems to be for constants, as far as I can tell in the discussion. I have had a reasonably good rummage and can't find anything covering variables.

---

_Comment by @MichaReiser on 2025-08-13 13:09_

I believe the underlying issue is the same: Ruff doesn't enforce a specific style for global variables.

---

_Comment by @t-m-sharpe on 2025-08-13 13:22_

What about instance variables defined inside `__init__`?

---

_Referenced in [astral-sh/ruff#20435](../../astral-sh/ruff/issues/20435.md) on 2025-09-16 16:06_

---
