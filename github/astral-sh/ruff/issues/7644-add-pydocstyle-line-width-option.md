---
number: 7644
title: "Add `pydocstyle.line-width` option"
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2023-09-25T09:05:07Z
updated_at: 2023-10-24T08:14:07Z
url: https://github.com/astral-sh/ruff/issues/7644
synced_at: 2026-01-07T13:12:15-06:00
---

# Add `pydocstyle.line-width` option

---

_Issue opened by @MichaReiser on 2023-09-25 09:05_

Add a new `pydocstyle.line-width` option that overrides the default `line-width` setting. This allows using the formatter with `E501` where `E501` uses another configured line-length. 

Considered alternatives: Creating a new option specific to pycodestyle rather than a formatter-specific option feels more intuitive because the override is specific to this lint rule and then aligns with pydocstyle.max-doc-length.

---

_Referenced in [astral-sh/ruff#7642](../../astral-sh/ruff/issues/7642.md) on 2023-09-25 09:10_

---

_Label `configuration` added by @MichaReiser on 2023-09-25 09:39_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:39_

---

_Referenced in [astral-sh/ruff#8039](../../astral-sh/ruff/pulls/8039.md) on 2023-10-18 08:53_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-19 01:26_

---

_Closed by @MichaReiser on 2023-10-24 08:14_

---
