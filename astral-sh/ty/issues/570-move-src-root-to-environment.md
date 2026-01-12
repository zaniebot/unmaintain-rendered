```yaml
number: 570
title: Move src.root to environment
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2025-06-02T19:53:01Z
updated_at: 2025-06-24T12:50:35Z
url: https://github.com/astral-sh/ty/issues/570
synced_at: 2026-01-12T15:54:23Z
```

# Move src.root to environment

---

_@MichaReiser_

The src.root setting configures the first party search path. It doesn't configure which files are checked. It, therefore, is a better fit for the environment section, similar to how pyright structures the option.

The setting may need a different name because root isn't very clear

---

_Added to milestone `Beta` by @MichaReiser on 2025-06-02 19:53_

---

_Label `configuration` added by @MichaReiser on 2025-06-02 19:53_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-06-18 14:18_

---

_Comment by @MichaReiser on 2025-06-24 12:50_

Closed by https://github.com/astral-sh/ruff/pull/18760

---

_Closed by @MichaReiser on 2025-06-24 12:50_

---
