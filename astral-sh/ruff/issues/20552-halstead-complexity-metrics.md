```yaml
number: 20552
title: Halstead complexity metrics
type: issue
state: closed
author: daanmlab
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-24T14:22:51Z
updated_at: 2025-09-24T16:18:10Z
url: https://github.com/astral-sh/ruff/issues/20552
synced_at: 2026-01-12T15:54:57Z
```

# Halstead complexity metrics

---

_@daanmlab_

### Background

Ruff already supports cyclomatic complexity (C90x) and cognitive complexity checks. These are valuable for identifying overly complex control flow, but they do not cover Halstead metrics, which measure program complexity based on operators and operands.

Halstead metrics are widely used in software engineering research and in some industry settings (e.g., as part of the maintainability index). Adding them to Ruff would provide a more complete complexity analysis alongside existing rules.

### Proposal

Introduce a new rule family for Halstead metrics, tentatively `HAL`:

`HAL001:` Halstead Volume exceeds configured maximum

`HAL002:` Halstead Effort exceeds configured maximum

`HAL003:` Halstead Difficulty exceeds configured maximum

### Configuration example (pyproject.toml):

```
[tool.ruff.lint.halstead]
max_volume = 800
max_effort = 20000
max_difficulty = 50

```

Example diagnostic:
```
HAL001 Function `calculate_metrics` has Halstead Volume 950 > 800
```
### Motivation

Today, Halstead metrics can only be obtained by running [Radon](https://pypi.org/project/radon)
 or other external tools. Integrating them directly into Ruff would:

Provide developers with a unified linting/complexity workflow

Avoid introducing another dependency

Benefit from Ruff’s speed and parallelism


---

_Label `rule` added by @ntBre on 2025-09-24 14:42_

---

_Label `needs-decision` added by @ntBre on 2025-09-24 14:42_

---

_Comment by @MichaReiser on 2025-09-24 16:18_

Thanks for opening this issue. 

I'll merge this into https://github.com/astral-sh/ruff/issues/2418 as it is a request for a new complexity metric and we track in https://github.com/astral-sh/ruff/issues/2418 the design for how we want to support multiple complexity metrics in Ruff

---

_Closed by @MichaReiser on 2025-09-24 16:18_

---
