---
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
synced_at: 2026-01-07T13:12:15-06:00
---

# Rename `line-length` to `line-width`

---

_Issue opened by @MichaReiser on 2023-09-21 11:36_

Rename the `line-length` setting to `line-width`. Update the documentation to mention that is also used for the formatter and that it is the line **width** and not **length**. 

Support `line-length` for an interim period. Warn when users use `line-length`. 

Note: We may want to consider adding an `alias` support to our `option` macro.

Maybe: Update the lint and remove/replace the `characters` in the message because it isn't measuring characters. 

---

_Referenced in [astral-sh/ruff#7642](../../astral-sh/ruff/issues/7642.md) on 2023-09-25 09:10_

---

_Label `configuration` added by @MichaReiser on 2023-09-25 09:24_

---

_Label `formatter` added by @MichaReiser on 2023-09-25 09:24_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:38_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-17 10:47_

---

_Referenced in [astral-sh/ruff#8010](../../astral-sh/ruff/pulls/8010.md) on 2023-10-18 08:21_

---

_Referenced in [astral-sh/ruff#8150](../../astral-sh/ruff/pulls/8150.md) on 2023-10-24 02:26_

---

_Closed by @MichaReiser on 2023-10-24 07:55_

---
