```yaml
number: 7573
title: "Rename `line-length` to `line-width`"
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
  - formatter
assignees: []
created_at: 2023-09-21T11:36:58Z
updated_at: 2023-10-24T07:55:23Z
url: https://github.com/astral-sh/ruff/issues/7573
synced_at: 2026-01-12T15:54:47Z
```

# Rename `line-length` to `line-width`

---

_@MichaReiser_

Rename the `line-length` setting to `line-width`. Update the documentation to mention that is also used for the formatter and that it is the line **width** and not **length**. 

Support `line-length` for an interim period. Warn when users use `line-length`. 

Note: We may want to consider adding an `alias` support to our `option` macro.

Maybe: Update the lint and remove/replace the `characters` in the message because it isn't measuring characters. 

---

_Label `configuration` added by @MichaReiser on 2023-09-25 09:24_

---

_Label `formatter` added by @MichaReiser on 2023-09-25 09:24_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:38_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-17 10:47_

---

_Closed by @MichaReiser on 2023-10-24 07:55_

---
