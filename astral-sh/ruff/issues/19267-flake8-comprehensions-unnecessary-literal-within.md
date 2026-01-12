```yaml
number: 19267
title: "[flake8-comprehensions] Unnecessary literal within `set` call"
type: issue
state: open
author: bluetech
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-10T17:21:33Z
updated_at: 2025-07-11T20:14:08Z
url: https://github.com/astral-sh/ruff/issues/19267
synced_at: 2026-01-12T15:54:56Z
```

# [flake8-comprehensions] Unnecessary literal within `set` call

---

_@bluetech_

### Summary

Currently there is no rule which warns about these:

```py
set({1, 2, 3})
set({x for x in range(10)})
```

The `set()` wrapper can just be removed because the argument is already a set.

I suggest adding a rule `unnecessary-literal-within-set-call`, following the example of [unnecessary-literal-within-dict-call](https://docs.astral.sh/ruff/rules/unnecessary-literal-within-dict-call/). 

---

_Label `rule` added by @ntBre on 2025-07-10 18:02_

---

_Label `needs-decision` added by @ntBre on 2025-07-10 18:02_

---

_Comment by @MichaReiser on 2025-07-11 16:42_

https://github.com/astral-sh/ruff/issues/18331 seems related

---

_Comment by @bluetech on 2025-07-11 20:14_

> #18331 seems related

Indeed, I was actually starting to write a rule for that issue, but then noticed this bigger "omission" and decided to suggest a fix for it first. Maybe #18331 can be lumped into this rule later, though it doesn't *exactly* fit.

---
