```yaml
number: 2001
title: "Rename `invalid-method-override` rule"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-12-17T11:58:23Z
updated_at: 2025-12-17T11:58:23Z
url: https://github.com/astral-sh/ty/issues/2001
synced_at: 2026-01-10T01:55:00Z
```

# Rename `invalid-method-override` rule

---

_Issue opened by @AlexWaygood on 2025-12-17 11:58_

We have lots of rules that complain about invalid method overrides of one sort or another, and we'll be adding more in the future. The `invalid-method-override` rule is specifically about Liskov violations, but its name doesn't reflect that currently. `unsound-method-override` might be better, though it's still not great. Maybe `incompatible-method-override`?

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 11:58_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-17 11:58_

---
