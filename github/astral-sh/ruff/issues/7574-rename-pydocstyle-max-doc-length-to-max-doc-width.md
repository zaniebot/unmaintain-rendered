---
number: 7574
title: "Rename `pydocstyle.max-doc-length` to `max-doc-width`"
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2023-09-21T11:37:14Z
updated_at: 2023-10-24T07:55:22Z
url: https://github.com/astral-sh/ruff/issues/7574
synced_at: 2026-01-07T13:12:15-06:00
---

# Rename `pydocstyle.max-doc-length` to `max-doc-width`

---

_Issue opened by @MichaReiser on 2023-09-21 11:37_

Rename `pydocstyle.max-doc-length` to `doc-width` to align with other text width options and clarify that the option is about the text width and not character or byte length. 

The `max-doc-length` option should act as an alias but Ruff should warn and hint users towards migrating to the new option.

---

_Referenced in [astral-sh/ruff#7642](../../astral-sh/ruff/issues/7642.md) on 2023-09-25 09:10_

---

_Label `configuration` added by @MichaReiser on 2023-09-25 09:28_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:38_

---

_Comment by @ngnpope on 2023-09-26 17:56_

Wondering whether the `max-` prefix is relevant? 🤔

If we are going for `line-width`, as the general line width value (without `max-`), would `doc-line-width` make more sense?

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-17 10:48_

---

_Referenced in [astral-sh/ruff#8010](../../astral-sh/ruff/pulls/8010.md) on 2023-10-18 08:21_

---

_Referenced in [astral-sh/ruff#8150](../../astral-sh/ruff/pulls/8150.md) on 2023-10-24 02:26_

---

_Closed by @MichaReiser on 2023-10-24 07:55_

---
