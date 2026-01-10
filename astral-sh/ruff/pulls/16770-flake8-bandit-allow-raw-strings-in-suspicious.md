```yaml
number: 16770
title: "[`flake8-bandit`] Allow raw strings in `suspicious-mark-safe-usage` (`S308`) #16702"
type: pull_request
state: merged
author: mfontanaar
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: s308/allow_string_literals
created_at: 2025-03-15T20:26:39Z
updated_at: 2025-03-17T10:29:07Z
url: https://github.com/astral-sh/ruff/pull/16770
synced_at: 2026-01-10T19:49:02Z
```

# [`flake8-bandit`] Allow raw strings in `suspicious-mark-safe-usage` (`S308`) #16702

---

_Pull request opened by @mfontanaar on 2025-03-15 20:26_

## Summary
Stop flagging each invocation of `django.utils.safestring.mark_safe` (also available at, `django.utils.html.mark_safe`) as an error.

Instead, allow string literals as valid uses for `mark_safe`.

Also, update the documentation, pointing at `django.utils.html.format_html` for dynamic content generation use cases.

Closes #16702 

## Test Plan
I verified several possible uses, but string literals, are still flagged.

---

_Label `rule` added by @ntBre on 2025-03-15 22:30_

---

_Renamed from "[flake8-bandit] Allow raw strings in `suspicious-mark-safe-usage` (S308) #16702" to "[`flake8-bandit`] Allow raw strings in `suspicious-mark-safe-usage` (`S308`) #16702" by @mfontanaar on 2025-03-16 16:10_

---

_@MichaReiser approved on 2025-03-17 08:21_

Thanks and I like the added reference to `format_html`

---

_Label `bug` added by @MichaReiser on 2025-03-17 08:21_

---

_Closed by @MichaReiser on 2025-03-17 08:21_

---

_Reopened by @MichaReiser on 2025-03-17 08:21_

---

_Comment by @github-actions[bot] on 2025-03-17 09:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2025-03-17 10:29_

---

_Closed by @MichaReiser on 2025-03-17 10:29_

---
