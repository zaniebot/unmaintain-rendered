```yaml
number: 18378
title: "Ruleset Suggestion: flake8-author"
type: issue
state: open
author: jeaboswell
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2025-05-30T01:33:08Z
updated_at: 2025-05-30T01:41:05Z
url: https://github.com/astral-sh/ruff/issues/18378
synced_at: 2026-01-12T15:54:56Z
```

# Ruleset Suggestion: flake8-author

---

_@jeaboswell_

### Summary

[`flake8-author`](https://github.com/jparise/flake8-author) is a [Flake8](http://flake8.pycqa.org/) extension that checks Python modules for module-level __author__ attributes.

# Error Codes

- `A400`: a module-level `__author__` attribute is required
- `A401`: `__author__` attributes are not allowed
- `A402`: `__author__` attribute value does not match _pattern_

# Configuration

- `author-attribute`: "optional", "required", "forbidden" (default: optional)
- `author-pattern`: `__author__` validation [re](https://docs.python.org/library/re.html) pattern (default: `''`)

---

_Label `plugin` added by @ntBre on 2025-05-30 01:41_

---

_Label `needs-decision` added by @ntBre on 2025-05-30 01:41_

---
