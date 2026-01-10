```yaml
number: 19359
title: "Add `from __future__ import annotations` in more rules"
type: issue
state: open
author: ntBre
labels:
  - rule
  - tracking
assignees: []
created_at: 2025-07-15T15:20:05Z
updated_at: 2025-07-15T15:27:36Z
url: https://github.com/astral-sh/ruff/issues/19359
synced_at: 2026-01-10T11:09:59Z
```

# Add `from __future__ import annotations` in more rules

---

_Issue opened by @ntBre on 2025-07-15 15:20_

https://github.com/astral-sh/ruff/pull/19100 added support for importing future annotations for TC001, TC002, TC003, UP037, and RUF013 as a proof of concept, but other rules could also be affected. In particular,

- [non-pep585-annotation (UP006)](https://docs.astral.sh/ruff/rules/non-pep585-annotation/#non-pep585-annotation-up006)
- [non-pep604-annotation-union (UP007)](https://docs.astral.sh/ruff/rules/non-pep604-annotation-union/)
- [non-pep604-annotation-optional (UP045)](https://docs.astral.sh/ruff/rules/non-pep604-annotation-optional/)

all currently rely on [future-rewritable-type-annotation (FA100)](https://docs.astral.sh/ruff/rules/future-rewritable-type-annotation/) for similar behavior,  but they could just add the import themselves. **This would likely mean deprecating FA100 eventually**.

Similarly, [runtime-import-in-type-checking-block (TC004)](https://docs.astral.sh/ruff/rules/runtime-import-in-type-checking-block/#runtime-import-in-type-checking-block-tc004) uses much of the same infrastructure as TC001-003. Adding a future import could be another option instead of `Action::Quote` in its implementation (the whole `Action` enum can likely be replaced by the new `TypingReference` enum).

[runtime-string-union (TC010)](https://docs.astral.sh/ruff/rules/runtime-string-union/#runtime-string-union-tc010) could also offer a fix by unquoting the flagged annotation and adding the future import.

## Status 

- [ ] UP006
- [ ] UP007
- [ ] UP045
- [ ] TC004
- [ ] TC010

---

_Label `rule` added by @ntBre on 2025-07-15 15:20_

---

_Label `tracking` added by @ntBre on 2025-07-15 15:27_

---
