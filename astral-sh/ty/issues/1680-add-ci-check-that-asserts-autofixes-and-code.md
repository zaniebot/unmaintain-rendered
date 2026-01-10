```yaml
number: 1680
title: Add CI check that asserts autofixes and code actions do not introduce invalid syntax
type: issue
state: open
author: AlexWaygood
labels:
  - testing
  - fixes
assignees: []
created_at: 2025-11-29T16:09:26Z
updated_at: 2025-11-29T16:09:26Z
url: https://github.com/astral-sh/ty/issues/1680
synced_at: 2026-01-10T01:58:59Z
```

# Add CI check that asserts autofixes and code actions do not introduce invalid syntax

---

_Issue opened by @AlexWaygood on 2025-11-29 16:09_

Ruff has a CI check that fails if any autofixes introduce invalid syntax. But our CI doesn't fail currently if a ty autofix or code action introduces invalid syntax. This will become much more important if we add a `--fix` CLI option (or `ty fix` command, etc.).

---

_Label `testing` added by @AlexWaygood on 2025-11-29 16:09_

---

_Label `fixes` added by @AlexWaygood on 2025-11-29 16:09_

---
