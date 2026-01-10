---
number: 18379
title: "Ruleset Suggestion: flake8-dunder-all"
type: issue
state: open
author: jeaboswell
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-05-30T01:40:31Z
updated_at: 2025-05-30T01:55:44Z
url: https://github.com/astral-sh/ruff/issues/18379
synced_at: 2026-01-10T01:22:59Z
---

# Ruleset Suggestion: flake8-dunder-all

---

_Issue opened by @jeaboswell on 2025-05-30 01:40_

### Summary

[`flake8-dunder-all`](https://github.com/python-formate/flake8-dunder-all) is a Flake8 plugin and pre-commit hook which checks to ensure modules have defined `__all__`.

# Error Codes

- `DALL000`: Module lacks `__all__`

---

_Comment by @ntBre on 2025-05-30 01:52_

It might be a bit much to add a new prefix for one rule. It looks like the other DALL [codes](https://flake8-dunder-all.readthedocs.io/en/latest/usage.html#flake8-codes) are covered by [unsorted-dunder-all](https://docs.astral.sh/ruff/rules/unsorted-dunder-all/)  and [invalid-all-format](https://docs.astral.sh/ruff/rules/invalid-all-format/), as you probably noticed. We could probably consider this as a standalone RUFF rule, though.

---

_Label `rule` added by @ntBre on 2025-05-30 01:52_

---

_Label `needs-decision` added by @ntBre on 2025-05-30 01:52_

---

_Comment by @jeaboswell on 2025-05-30 01:55_

Yeah, I kinda figured it would make more sense to add it as a RUFF rule.  I just added it this way to more show where the rule originated, at least for me.

---
