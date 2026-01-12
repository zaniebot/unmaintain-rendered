```yaml
number: 7648
title: "Use `tab_width` option in formatter"
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
  - formatter
assignees: []
created_at: 2023-09-25T09:20:29Z
updated_at: 2023-10-18T23:48:17Z
url: https://github.com/astral-sh/ruff/issues/7648
synced_at: 2026-01-12T15:54:47Z
```

# Use `tab_width` option in formatter

---

_@MichaReiser_

Allow customizing the `tab_width` in the formatter by respecting the global `tab_width` (`tab-size` today) option. 

The `tab_width` should only change the width of a tab character but not allow changing the number of spaces when using `indent-style= "space"` (May require reverting https://github.com/astral-sh/ruff/pull/7301)

---

_Label `configuration` added by @MichaReiser on 2023-09-25 09:20_

---

_Label `formatter` added by @MichaReiser on 2023-09-25 09:20_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:40_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-17 10:14_

---

_Closed by @MichaReiser on 2023-10-18 23:48_

---
