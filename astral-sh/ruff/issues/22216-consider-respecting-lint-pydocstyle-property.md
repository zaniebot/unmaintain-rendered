---
number: 22216
title: "Consider respecting `lint.pydocstyle.property-decorators` in `RUF066`"
type: issue
state: open
author: ntBre
labels:
  - rule
  - configuration
  - preview
assignees: []
created_at: 2025-12-26T23:16:00Z
updated_at: 2025-12-26T23:16:00Z
url: https://github.com/astral-sh/ruff/issues/22216
synced_at: 2026-01-10T01:23:02Z
---

# Consider respecting `lint.pydocstyle.property-decorators` in `RUF066`

---

_Issue opened by @ntBre on 2025-12-26 23:16_

The other references to this `is_property` function end up passing the [property-decorators](https://docs.astral.sh/ruff/settings/#lint_pydocstyle_property-decorators) setting, so I think we should do the same here.

https://github.com/astral-sh/ruff/blob/95a532f9fd67273d93ebcfb5bc09537f60404244/crates/ruff_linter/src/rules/ruff/rules/property_without_return.rs#L65

---

_Label `rule` added by @ntBre on 2025-12-26 23:16_

---

_Label `configuration` added by @ntBre on 2025-12-26 23:16_

---

_Label `preview` added by @ntBre on 2025-12-26 23:16_

---
