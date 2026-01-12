```yaml
number: 16706
title: "PGH004: Check for missing colons in file-level suppression comments"
type: issue
state: open
author: MichaReiser
labels:
  - rule
  - diagnostics
assignees: []
created_at: 2025-03-13T12:45:12Z
updated_at: 2025-03-13T12:50:17Z
url: https://github.com/astral-sh/ruff/issues/16706
synced_at: 2026-01-12T15:54:55Z
```

# PGH004: Check for missing colons in file-level suppression comments

---

_@MichaReiser_

This but for file-level suppression comments

https://github.com/astral-sh/ruff/blob/b8f128442e64dd3349835506978dc7fa9d58ccd3/crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs#L113-L125


This has come up in https://github.com/astral-sh/ruff/pull/16699

> Nice! I do think that we should have a followup issue/PR where we add the feature that this rule has for inline suppressions where it tries to guess if you just forgot a colon



---

_Label `rule` added by @MichaReiser on 2025-03-13 12:45_

---

_Label `diagnostics` added by @dylwil3 on 2025-03-13 12:49_

---

_Comment by @dylwil3 on 2025-03-13 12:50_

Note that this would not change the logic for when the diagnostic is emitted, only for what the diagnostic says.

---
