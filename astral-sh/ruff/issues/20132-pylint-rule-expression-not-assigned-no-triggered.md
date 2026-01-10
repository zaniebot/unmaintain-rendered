```yaml
number: 20132
title: Pylint rule expression-not-assigned no triggered on list comprehension W0106 (B018)
type: issue
state: open
author: kaddkaka
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-28T12:06:51Z
updated_at: 2025-09-15T13:20:48Z
url: https://github.com/astral-sh/ruff/issues/20132
synced_at: 2026-01-10T11:09:59Z
```

# Pylint rule expression-not-assigned no triggered on list comprehension W0106 (B018)

---

_Issue opened by @kaddkaka on 2025-08-28 12:06_

### Summary

Pylint W0106 is triggered on each line of this code:

```py
[x + 1 for x in range(10)]
[1 for _ in range(2)]
[x in range(5) if x > 3 else 0 for x in range(10)]
```

But ruffs corresponding rule `B018` is not triggered on either.

### Version

ruff 0.11.13

---

_Comment by @kaddkaka on 2025-08-28 12:09_

```py
[100]
```

triggers:
- pylint `W0104` "pointless-statement"
- ruff `B018` "Found useless expresion"

Is `B018` an alias of W0104, W0106, or both?

---

_Comment by @ntBre on 2025-08-28 15:29_

It looks like these are getting caught by the `contains_effect` check in B018, which marks all comprehensions, among other expressions, as possibly containing side effects:

https://github.com/astral-sh/ruff/blob/9930c94c0971487cb3f1ab9bbc7230acac717aec/crates/ruff_linter/src/rules/flake8_bugbear/rules/useless_expression.rs#L98-L99

https://github.com/astral-sh/ruff/blob/9930c94c0971487cb3f1ab9bbc7230acac717aec/crates/ruff_python_ast/src/helpers.rs#L114-L121

so this looks like an intentional decision to err on the side of false negatives to me. There was some discussion of alternatives in https://github.com/astral-sh/ruff/pull/3455, though.

---

_Label `question` added by @ntBre on 2025-08-28 15:29_

---

_Label `question` removed by @MichaReiser on 2025-09-15 13:20_

---

_Label `rule` added by @MichaReiser on 2025-09-15 13:20_

---

_Label `needs-decision` added by @MichaReiser on 2025-09-15 13:20_

---
