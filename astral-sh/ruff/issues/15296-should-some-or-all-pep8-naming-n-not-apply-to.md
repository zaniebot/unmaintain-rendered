```yaml
number: 15296
title: "Should some (or all) `pep8-naming (N)` not apply to stubs?"
type: issue
state: open
author: Avasam
labels:
  - question
  - rule
  - needs-decision
assignees: []
created_at: 2025-01-06T04:32:21Z
updated_at: 2025-01-06T08:04:19Z
url: https://github.com/astral-sh/ruff/issues/15296
synced_at: 2026-01-12T15:54:54Z
```

# Should some (or all) `pep8-naming (N)` not apply to stubs?

---

_@Avasam_

As @harahu mentioned in https://github.com/astral-sh/ruff/issues/14535#issuecomment-2516967026, most pep8-naming rules don't apply for third-party stubs
  - **But** relevant rules could be made to apply to names decorated with `@type_check_only`, since that's a clear signal the stub author is very likely in control of that name (there's some niche exceptions for when a class is defined inside a function, but needs to be exposed in stubs)
  - `N811`, `N812`, `N813`, `N814`, `N817` are probably fine in stubs, as long as the name isn't re-exported (in `__all__`, `foo as foo` or `var = var`)

Ruff: 0.8.5

(this report is extracted from https://github.com/astral-sh/ruff/issues/14535#issuecomment-2561461038 for ease of tracking and discussion)

---

_Label `question` added by @dhruvmanila on 2025-01-06 05:49_

---

_Label `rule` added by @MichaReiser on 2025-01-06 08:04_

---

_Label `needs-decision` added by @MichaReiser on 2025-01-06 08:04_

---
