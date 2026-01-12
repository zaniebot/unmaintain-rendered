```yaml
number: 7742
title: "Extend `no-return-argument-annotation-in-stub` (`PYI050`) to all name annotations and non-stub files"
type: issue
state: open
author: tjkuson
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-10-01T18:01:27Z
updated_at: 2025-12-29T23:43:48Z
url: https://github.com/astral-sh/ruff/issues/7742
synced_at: 2026-01-12T15:54:47Z
```

# Extend `no-return-argument-annotation-in-stub` (`PYI050`) to all name annotations and non-stub files

---

_@tjkuson_

The given reason for preferring `typing.Never` to `typing.NoReturn` for argument type annotations is that the name is more explicit about the annotation's intent. 

Currently, this rule is limited to function arguments in stub files. Following the reasoning of the rule, it should probably also apply to other variable annotations and in non-stub files.

---

_Label `rule` added by @ntBre on 2025-12-29 23:43_

---

_Label `needs-decision` added by @ntBre on 2025-12-29 23:43_

---
