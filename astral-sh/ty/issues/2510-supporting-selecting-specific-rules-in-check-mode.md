```yaml
number: 2510
title: Supporting selecting specific rules in check mode
type: issue
state: closed
author: Feiyang472
labels:
  - question
  - cli
  - configuration
assignees: []
created_at: 2026-01-15T10:42:34Z
updated_at: 2026-01-15T12:18:41Z
url: https://github.com/astral-sh/ty/issues/2510
synced_at: 2026-01-15T12:52:53Z
```

# Supporting selecting specific rules in check mode

---

_@Feiyang472_

### Question

ty version 0.0.12 check command supports `ignore, warn, error` flags to alter rule behaviour. It would be great to support selecting a whitelist of rules like `ruff check --select` to enable. Are there plans to add this functionality? 

### Version

0.0.12

---

_Label `question` added by @Feiyang472 on 2026-01-15 10:42_

---

_Label `cli` added by @AlexWaygood on 2026-01-15 11:01_

---

_Label `configuration` added by @AlexWaygood on 2026-01-15 11:01_

---

_Comment by @MichaReiser on 2026-01-15 12:18_

Not directly, see https://github.com/astral-sh/ty/issues/2066 for some context. But we plan on introducing something like an `ALL` selector (https://github.com/astral-sh/ty/issues/174) so that you can deselect all default rules

---

_Closed by @MichaReiser on 2026-01-15 12:18_

---
